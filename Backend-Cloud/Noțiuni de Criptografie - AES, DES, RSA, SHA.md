### **Concepte de Bază în Criptografie**

**Criptografia** = știința protejării informației prin transformarea ei într-o formă ilizibilă (criptare) și recuperarea formei originale (decriptare).

**Două tipuri principale de criptare:**

- **Criptare simetrică** - aceeași cheie pentru criptare și decriptare
- **Criptare asimetrică** - cheie publică pentru criptare, cheie privată pentru decriptare

---

### **DES (Data Encryption Standard)**

**Tip:** Criptare simetrică (algoritm bloc)

**Caracteristici:**

- Dezvoltat în anii 1970, standardizat în 1977
- Folosește chei de **56 biți** (+ 8 biți paritate = 64 biți total)
- Criptează blocuri de **64 biți** de date
- **DEPRECIAT** - considerat nesigur din cauza cheii prea scurte

**Cum funcționează:**

- Aplică 16 runde de transformări asupra datelor
- Folosește substituții și permutări complexe
- Poate fi spart prin brute-force în câteva ore cu hardware modern

**Variante:**

- **3DES (Triple DES)** - aplică DES de 3 ori consecutiv
- Mai sigur dar mult mai lent decât alternativele moderne

---

### **AES (Advanced Encryption Standard)**

**Tip:** Criptare simetrică (algoritm bloc)

**Caracteristici:**

- Standardul actual (din 2001), înlocuitorul lui DES
- Chei de **128, 192 sau 256 biți**
- Criptează blocuri de **128 biți**
- Extrem de sigur și eficient

**Cum funcționează:**

```
Text clar → [AES + Cheie secretă] → Text criptat
Text criptat → [AES + Aceeași cheie] → Text clar
```

**Utilizări practice:**

- **WiFi WPA2/WPA3** - securizarea rețelelor wireless
- **HTTPS** - criptarea traficului web
- **Criptarea discurilor** - BitLocker, FileVault
- **VPN-uri** - tuneluri securizate
- **WhatsApp, Signal** - mesaje criptate end-to-end

**Avantaje:**

- Foarte rapid (optimizat hardware pe procesoare moderne)
- Rezistent la toate atacurile cunoscute
- Standard global acceptat

---

### **RSA (Rivest-Shamir-Adleman)**

**Tip:** Criptare asimetrică (cheie publică)

**Caracteristici:**

- Bazat pe dificultatea factorizării numerelor prime mari
- Chei tipice: **2048 sau 4096 biți**
- Mai lent decât AES (de ~1000 ori)
- Folosit pentru criptarea cheilor, nu a datelor mari

**Cum funcționează:**

```
Alice generează: Cheie Publică + Cheie Privată

Bob → criptează cu Cheia Publică a lui Alice → Alice decriptează cu Cheia ei Privată
Alice → semnează cu Cheia Privată → Bob verifică cu Cheia Publică a lui Alice
```

**Utilizări practice:**

- **Certificate SSL/TLS** - autentificarea site-urilor web
- **Semnături digitale** - verificarea autenticității documentelor
- **Schimb de chei** - transmiterea securizată a cheilor AES
- **Email criptat** - PGP/GPG
- **Autentificare SSH** - conectare securizată la servere

**Exemplu practic:** Când accesezi un site HTTPS:

1. Browserul primește certificatul cu cheia publică RSA
2. Browserul generează o cheie AES temporară
3. Criptează cheia AES cu RSA și o trimite serverului
4. Comunicarea continuă cu AES (mai rapid)

---

### **SHA (Secure Hash Algorithm)**

**Tip:** Funcție hash criptografică (NU este criptare!)

**Ce face o funcție hash:**

- Transformă orice input într-un output de dimensiune fixă
- **Unidirecțională** - imposibil de inversat
- **Deterministă** - același input = același hash
- **Avalanche effect** - o mică schimbare în input → hash complet diferit

**Familia SHA:**

- **SHA-1** (160 biți) - DEPRECIAT, vulnerabil la coliziuni
- **SHA-256** (256 biți) - standard actual, folosit în Bitcoin
- **SHA-512** (512 biți) - mai sigur, pentru aplicații critice
- **SHA-3** - cea mai nouă generație

**Exemple SHA-256:**

```
"Hello" → 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
"Hello!" → 334d016f755cd6dc58c53a86e183882f8ec14f52fb05345887c8a5edd42c87b7
```

**Utilizări practice:**

- **Stocarea parolelor** - nu se salvează parola, ci hash-ul ei
- **Verificarea integrității** - checksum pentru download-uri
- **Blockchain** - Bitcoin folosește SHA-256 pentru mining
- **Semnături digitale** - hash-ul documentului este semnat cu RSA
- **Certificate digitale** - verificarea autenticității

---

### **Cum Lucrează Împreună**

**Exemplu complet - Trimiterea unui mesaj securizat:**

1. **Alice vrea să trimită un document secret către Bob**
    
2. **Pregătirea:**
    
    - Alice calculează **SHA-256** al documentului (pentru integritate)
    - Alice semnează hash-ul cu **cheia ei privată RSA** (autenticitate)
    - Alice criptează documentul cu **AES-256** folosind o cheie aleatoare
    - Alice criptează cheia AES cu **cheia publică RSA** a lui Bob
3. **Trimiterea:**
    
    - Document criptat (AES)
    - Cheie AES criptată (RSA)
    - Semnătură digitală (RSA + SHA)
4. **Recepția - Bob:**
    
    - Decriptează cheia AES cu **cheia sa privată RSA**
    - Decriptează documentul cu cheia **AES**
    - Verifică semnătura cu **cheia publică** a lui Alice
    - Recalculează **SHA-256** pentru a verifica integritatea

### **Rezumat Comparativ**

|Algoritm|Tip|Viteză|Utilizare Principală|
|---|---|---|---|
|**DES**|Simetric|Rapid|DEPRECIAT - nu mai folosiți|
|**AES**|Simetric|Foarte rapid|Criptarea datelor mari|
|**RSA**|Asimetric|Lent|Schimb chei, semnături|
|**SHA**|Hash|Foarte rapid|Integritate, parole|

Aceste tehnologii formează fundamentul securității digitale moderne, protejând tot de la mesajele WhatsApp până la tranzacțiile bancare online.