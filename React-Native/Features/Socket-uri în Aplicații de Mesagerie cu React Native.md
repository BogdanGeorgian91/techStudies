### **Ce sunt WebSocket-urile?**

**WebSocket** = Protocol de comunicare **bidirecțională în timp real** între client și server, peste o singură conexiune TCP persistentă.

**Diferența față de HTTP clasic:**

```
HTTP Traditional:
Client → Request → Server
Client ← Response ← Server
(Conexiune închisă)

WebSocket:
Client ←→ Conexiune Persistentă ←→ Server
(Comunicare continuă în ambele direcții)
```

### **De ce WebSocket pentru Mesagerie?**

**Avantaje:**

- **Comunicare în timp real** - mesaje instant, fără polling
- **Bidirecțional** - serverul poate trimite date fără request
- **Eficient** - o singură conexiune pentru toată sesiunea
- **Low latency** - fără overhead-ul HTTP pentru fiecare mesaj

**Cazuri de utilizare:**

- Mesaje instant (WhatsApp, Messenger)
- Notificări în timp real
- Status online/offline
- Indicatori "typing..."
- Seen/Delivered status

---

## **Arhitectura Completă**

```
┌──────────────┐         WebSocket          ┌──────────────┐
│              │ ←─────────────────────────→ │              │
│ React Native │                             │ Node.js      │
│ Client       │    Socket.IO Protocol       │ Server       │
│              │ ←─────────────────────────→ │              │
└──────────────┘                             └──────────────┘
       ↓                                             ↓
   [Socket.IO                                   [Socket.IO
    Client]                                      Server]
                                                     ↓
                                               [MongoDB/
                                                PostgreSQL]
```

---

## **Implementare Practică**

### **1. SERVER - Node.js cu Socket.IO**

```javascript
// server.js
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server, {
  cors: {
    origin: "*",
    methods: ["GET", "POST"]
  }
});

// Stocăm utilizatorii conectați
const users = new Map();

io.on('connection', (socket) => {
  console.log('User connected:', socket.id);

  // 1. AUTENTIFICARE
  socket.on('authenticate', (userData) => {
    users.set(socket.id, {
      userId: userData.userId,
      username: userData.username,
      socketId: socket.id
    });

    // Adaugă user-ul în camera lui personală
    socket.join(`user_${userData.userId}`);
    
    // Notifică alți utilizatori că user-ul e online
    socket.broadcast.emit('userOnline', {
      userId: userData.userId,
      username: userData.username
    });
  });

  // 2. TRIMITERE MESAJ PRIVAT
  socket.on('sendMessage', async (data) => {
    const { recipientId, message, timestamp } = data;
    const sender = users.get(socket.id);

    // Salvează mesajul în baza de date
    const savedMessage = {
      id: generateId(),
      senderId: sender.userId,
      recipientId: recipientId,
      message: message,
      timestamp: timestamp,
      status: 'sent'
    };

    // Trimite mesajul către destinatar
    io.to(`user_${recipientId}`).emit('receiveMessage', {
      ...savedMessage,
      senderName: sender.username
    });

    // Confirmă trimiterea către expeditor
    socket.emit('messageDelivered', {
      messageId: savedMessage.id,
      status: 'delivered'
    });
  });

  // 3. TYPING INDICATOR
  socket.on('typing', ({ recipientId, isTyping }) => {
    const sender = users.get(socket.id);
    
    io.to(`user_${recipientId}`).emit('userTyping', {
      userId: sender.userId,
      username: sender.username,
      isTyping: isTyping
    });
  });

  // 4. JOIN CHAT ROOM (pentru grupuri)
  socket.on('joinRoom', (roomId) => {
    socket.join(`room_${roomId}`);
    
    // Notifică alți membri din grup
    socket.to(`room_${roomId}`).emit('userJoinedRoom', {
      userId: users.get(socket.id).userId,
      roomId: roomId
    });
  });

  // 5. MESAJ ÎN GRUP
  socket.on('sendGroupMessage', (data) => {
    const { roomId, message } = data;
    const sender = users.get(socket.id);

    // Trimite la toți din cameră, EXCLUSIV expeditorul
    socket.to(`room_${roomId}`).emit('receiveGroupMessage', {
      roomId: roomId,
      message: message,
      senderId: sender.userId,
      senderName: sender.username,
      timestamp: new Date()
    });
  });

  // 6. MESSAGE STATUS UPDATES
  socket.on('messageRead', ({ messageId, senderId }) => {
    // Notifică expeditorul că mesajul a fost citit
    io.to(`user_${senderId}`).emit('messageStatusUpdate', {
      messageId: messageId,
      status: 'read',
      readAt: new Date()
    });
  });

  // 7. DECONECTARE
  socket.on('disconnect', () => {
    const user = users.get(socket.id);
    if (user) {
      // Notifică că user-ul e offline
      socket.broadcast.emit('userOffline', {
        userId: user.userId
      });
      users.delete(socket.id);
    }
    console.log('User disconnected:', socket.id);
  });
});

server.listen(3000, () => {
  console.log('Socket server running on port 3000');
});
```

---

### **2. CLIENT - React Native**

```javascript
// socketService.js - Serviciu centralizat pentru Socket
import io from 'socket.io-client';

class SocketService {
  constructor() {
    this.socket = null;
    this.listeners = new Map();
  }

  // CONECTARE LA SERVER
  connect(serverUrl, userId, username) {
    return new Promise((resolve, reject) => {
      this.socket = io(serverUrl, {
        transports: ['websocket'],
        reconnection: true,
        reconnectionDelay: 1000,
        reconnectionAttempts: 5
      });

      this.socket.on('connect', () => {
        console.log('Connected to server');
        
        // Autentifică user-ul
        this.socket.emit('authenticate', {
          userId: userId,
          username: username
        });
        
        resolve(this.socket);
      });

      this.socket.on('connect_error', (error) => {
        console.log('Connection error:', error);
        reject(error);
      });

      // RECONECTARE AUTOMATĂ
      this.socket.on('reconnect', (attemptNumber) => {
        console.log('Reconnected after', attemptNumber, 'attempts');
        // Re-autentificare după reconectare
        this.socket.emit('authenticate', { userId, username });
      });

      this.socket.on('disconnect', (reason) => {
        console.log('Disconnected:', reason);
      });
    });
  }

  // TRIMITERE MESAJ
  sendMessage(recipientId, message) {
    if (this.socket) {
      this.socket.emit('sendMessage', {
        recipientId: recipientId,
        message: message,
        timestamp: new Date()
      });
    }
  }

  // TYPING INDICATOR
  sendTypingStatus(recipientId, isTyping) {
    if (this.socket) {
      this.socket.emit('typing', {
        recipientId: recipientId,
        isTyping: isTyping
      });
    }
  }

  // JOIN ROOM
  joinRoom(roomId) {
    if (this.socket) {
      this.socket.emit('joinRoom', roomId);
    }
  }

  // LISTENERS
  on(event, callback) {
    if (this.socket) {
      this.socket.on(event, callback);
      // Salvăm listener-ul pentru cleanup
      if (!this.listeners.has(event)) {
        this.listeners.set(event, []);
      }
      this.listeners.get(event).push(callback);
    }
  }

  // CLEANUP
  removeListener(event, callback) {
    if (this.socket) {
      this.socket.off(event, callback);
    }
  }

  disconnect() {
    if (this.socket) {
      this.socket.disconnect();
      this.socket = null;
    }
  }
}

export default new SocketService();
```

---

### **3. COMPONENTA CHAT - React Native**

```javascript
// ChatScreen.js
import React, { useState, useEffect, useRef } from 'react';
import {
  View,
  Text,
  TextInput,
  FlatList,
  TouchableOpacity,
  KeyboardAvoidingView,
  Platform
} from 'react-native';
import SocketService from './socketService';
import AsyncStorage from '@react-native-async-storage/async-storage';

function ChatScreen({ route }) {
  const { recipientId, recipientName } = route.params;
  const [messages, setMessages] = useState([]);
  const [inputMessage, setInputMessage] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const [recipientTyping, setRecipientTyping] = useState(false);
  const [connectionStatus, setConnectionStatus] = useState('connecting');
  const typingTimer = useRef(null);
  const flatListRef = useRef(null);

  useEffect(() => {
    // Conectare la socket server
    initializeSocket();

    // Încarcă mesajele anterioare din storage/API
    loadPreviousMessages();

    // Cleanup la demontare
    return () => {
      SocketService.removeListener('receiveMessage');
      SocketService.removeListener('userTyping');
      SocketService.removeListener('messageDelivered');
      SocketService.removeListener('messageStatusUpdate');
    };
  }, []);

  const initializeSocket = async () => {
    try {
      const userId = await AsyncStorage.getItem('userId');
      const username = await AsyncStorage.getItem('username');

      await SocketService.connect('http://localhost:3000', userId, username);
      setConnectionStatus('connected');

      // ASCULTĂ MESAJE NOI
      SocketService.on('receiveMessage', (message) => {
        setMessages(prev => [...prev, {
          ...message,
          type: 'received'
        }]);
        
        // Trimite confirmare de citire
        SocketService.socket.emit('messageRead', {
          messageId: message.id,
          senderId: message.senderId
        });

        // Scroll automat la mesajul nou
        flatListRef.current?.scrollToEnd();
      });

      // ASCULTĂ TYPING INDICATOR
      SocketService.on('userTyping', (data) => {
        if (data.userId === recipientId) {
          setRecipientTyping(data.isTyping);
          
          // Auto-hide după 3 secunde
          if (data.isTyping) {
            setTimeout(() => setRecipientTyping(false), 3000);
          }
        }
      });

      // CONFIRMARE LIVRARE MESAJ
      SocketService.on('messageDelivered', (data) => {
        setMessages(prev => prev.map(msg => 
          msg.id === data.messageId 
            ? { ...msg, status: 'delivered' } 
            : msg
        ));
      });

      // STATUS MESAJ CITIT
      SocketService.on('messageStatusUpdate', (data) => {
        setMessages(prev => prev.map(msg => 
          msg.id === data.messageId 
            ? { ...msg, status: data.status, readAt: data.readAt } 
            : msg
        ));
      });

    } catch (error) {
      console.error('Socket connection failed:', error);
      setConnectionStatus('error');
    }
  };

  const loadPreviousMessages = async () => {
    try {
      // Încarcă din API sau local storage
      const response = await fetch(`/api/messages/${recipientId}`);
      const previousMessages = await response.json();
      setMessages(previousMessages);
    } catch (error) {
      console.error('Failed to load messages:', error);
    }
  };

  const sendMessage = () => {
    if (inputMessage.trim()) {
      const newMessage = {
        id: Date.now().toString(),
        text: inputMessage,
        senderId: 'currentUser',
        type: 'sent',
        timestamp: new Date(),
        status: 'sending'
      };

      // Adaugă mesajul local instant
      setMessages(prev => [...prev, newMessage]);
      
      // Trimite prin socket
      SocketService.sendMessage(recipientId, inputMessage);
      
      // Clear input
      setInputMessage('');
      
      // Stop typing indicator
      handleTypingStop();
    }
  };

  const handleTyping = (text) => {
    setInputMessage(text);

    // Trimite typing indicator
    if (!isTyping && text.length > 0) {
      setIsTyping(true);
      SocketService.sendTypingStatus(recipientId, true);
    }

    // Reset timer
    clearTimeout(typingTimer.current);
    typingTimer.current = setTimeout(() => {
      handleTypingStop();
    }, 1000);
  };

  const handleTypingStop = () => {
    if (isTyping) {
      setIsTyping(false);
      SocketService.sendTypingStatus(recipientId, false);
    }
    clearTimeout(typingTimer.current);
  };

  const renderMessage = ({ item }) => {
    const isSent = item.type === 'sent';
    
    return (
      <View style={[
        styles.messageContainer,
        isSent ? styles.sentMessage : styles.receivedMessage
      ]}>
        <Text style={styles.messageText}>{item.text}</Text>
        <View style={styles.messageFooter}>
          <Text style={styles.timestamp}>
            {new Date(item.timestamp).toLocaleTimeString()}
          </Text>
          {isSent && (
            <Text style={styles.status}>
              {item.status === 'read' ? '✓✓' : 
               item.status === 'delivered' ? '✓✓' : '✓'}
            </Text>
          )}
        </View>
      </View>
    );
  };

  return (
    <KeyboardAvoidingView 
      style={styles.container}
      behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
    >
      {/* Header cu status conexiune */}
      <View style={styles.header}>
        <Text style={styles.headerTitle}>{recipientName}</Text>
        <Text style={styles.connectionStatus}>
          {connectionStatus === 'connected' ? '🟢' : '🔴'}
        </Text>
      </View>

      {/* Typing Indicator */}
      {recipientTyping && (
        <View style={styles.typingIndicator}>
          <Text>{recipientName} is typing...</Text>
        </View>
      )}

      {/* Lista de mesaje */}
      <FlatList
        ref={flatListRef}
        data={messages}
        renderItem={renderMessage}
        keyExtractor={item => item.id}
        onContentSizeChange={() => flatListRef.current?.scrollToEnd()}
      />

      {/* Input pentru mesaj */}
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          value={inputMessage}
          onChangeText={handleTyping}
          onBlur={handleTypingStop}
          placeholder="Type a message..."
          multiline
        />
        <TouchableOpacity 
          style={styles.sendButton}
          onPress={sendMessage}
        >
          <Text style={styles.sendButtonText}>Send</Text>
        </TouchableOpacity>
      </View>
    </KeyboardAvoidingView>
  );
}
```

---

## **Gestionarea Conexiunii și Reconectare**

```javascript
// ConnectionManager.js
class ConnectionManager {
  constructor() {
    this.reconnectAttempts = 0;
    this.maxReconnectAttempts = 5;
    this.reconnectDelay = 1000;
  }

  handleConnectionLoss() {
    // Strategie de reconectare exponențială
    const attemptReconnect = () => {
      if (this.reconnectAttempts < this.maxReconnectAttempts) {
        this.reconnectAttempts++;
        
        setTimeout(() => {
          console.log(`Reconnect attempt ${this.reconnectAttempts}`);
          
          SocketService.connect()
            .then(() => {
              this.reconnectAttempts = 0;
              this.onReconnectSuccess();
            })
            .catch(() => {
              // Dublează delay-ul pentru următoarea încercare
              this.reconnectDelay *= 2;
              attemptReconnect();
            });
        }, this.reconnectDelay);
      } else {
        this.onReconnectFailed();
      }
    };

    attemptReconnect();
  }

  onReconnectSuccess() {
    // Re-sync mesaje pierdute
    this.syncMissedMessages();
    // Actualizează UI
    EventEmitter.emit('connectionRestored');
  }

  syncMissedMessages() {
    // Obține ultimul timestamp cunoscut
    const lastMessageTime = this.getLastMessageTimestamp();
    
    // Cere serverului mesajele lipsă
    SocketService.socket.emit('syncMessages', {
      since: lastMessageTime
    });
  }
}
```

---

## **Optimizări și Best Practices**

### **1. Message Queue pentru Offline**

```javascript
class OfflineMessageQueue {
  constructor() {
    this.queue = [];
  }

  addMessage(message) {
    this.queue.push({
      ...message,
      queuedAt: new Date(),
      status: 'queued'
    });
    this.saveToStorage();
  }

  async sendQueuedMessages() {
    while (this.queue.length > 0) {
      const message = this.queue.shift();
      
      try {
        await SocketService.sendMessage(
          message.recipientId, 
          message.text
        );
      } catch (error) {
        // Re-adaugă în coadă dacă trimiterea eșuează
        this.queue.unshift(message);
        break;
      }
    }
  }

  async saveToStorage() {
    await AsyncStorage.setItem(
      'offlineQueue', 
      JSON.stringify(this.queue)
    );
  }
}
```

### **2. Compresie Mesaje**

```javascript
// Pentru mesaje mari sau imagini
import pako from 'pako';

const compressMessage = (message) => {
  const compressed = pako.deflate(JSON.stringify(message));
  return Buffer.from(compressed).toString('base64');
};

const decompressMessage = (compressedData) => {
  const buffer = Buffer.from(compressedData, 'base64');
  const decompressed = pako.inflate(buffer);
  return JSON.parse(new TextDecoder().decode(decompressed));
};
```

### **3. Heartbeat pentru Connection Health**

```javascript
class HeartbeatManager {
  startHeartbeat() {
    this.heartbeatInterval = setInterval(() => {
      if (SocketService.socket?.connected) {
        SocketService.socket.emit('ping');
        
        this.pongTimeout = setTimeout(() => {
          console.log('Connection lost - no pong received');
          this.handleConnectionLoss();
        }, 5000);
      }
    }, 30000); // La fiecare 30 secunde
  }

  handlePong() {
    clearTimeout(this.pongTimeout);
  }

  stopHeartbeat() {
    clearInterval(this.heartbeatInterval);
    clearTimeout(this.pongTimeout);
  }
}
```

---

## **Securitate**

```javascript
// 1. Autentificare cu JWT
socket.on('connection', async (socket) => {
  const token = socket.handshake.auth.token;
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    socket.userId = decoded.userId;
  } catch (error) {
    socket.disconnect();
    return;
  }
});

// 2. Rate Limiting
const rateLimiter = new Map();

socket.on('sendMessage', (data) => {
  const userRate = rateLimiter.get(socket.userId) || 0;
  
  if (userRate > 10) { // Max 10 mesaje/minut
    socket.emit('error', 'Rate limit exceeded');
    return;
  }
  
  rateLimiter.set(socket.userId, userRate + 1);
});

// 3. Validare și Sanitizare Input
const sanitizeMessage = (message) => {
  return message
    .trim()
    .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
    .slice(0, 1000); // Max 1000 caractere
};
```

---

## **Fluxul Complet al unui Mesaj**

```
1. User tastează → TextInput onChange
2. Typing indicator → Socket emit('typing')
3. User apasă Send → Mesaj adăugat local (optimistic UI)
4. Socket emit('sendMessage') → Server
5. Server salvează în DB
6. Server emit('receiveMessage') → Destinatar
7. Server emit('messageDelivered') → Expeditor
8. Destinatar vede mesajul → emit('messageRead')
9. Server emit('messageStatusUpdate') → Expeditor
10. UI actualizează status la "✓✓" (citit)
```

Această arhitectură oferă o experiență de chat în timp real, fluidă și fiabilă, similară cu aplicațiile moderne de mesagerie.