### **Diferența dintre Autentificare și Autorizare**

**Autentificarea** = Cine ești? (verificarea identității)

- Procesul prin care un sistem verifică că utilizatorul este cine pretinde că este
- Exemplu: login cu username și parolă

**Autorizarea** = Ce ai voie să faci? (verificarea permisiunilor)

- Procesul prin care se determină ce resurse și acțiuni are dreptul să acceseze un utilizator autentificat
- Exemplu: un utilizator normal nu poate accesa panoul de administrare

### **OAuth2 (Open Authorization 2.0)**

OAuth2 este un **protocol de autorizare** care permite aplicațiilor să obțină acces limitat la resursele unui utilizator, fără a-i expune credențialele.

**Cum funcționează:**

- Permite unei aplicații terțe să acceseze resurse în numele utilizatorului
- Utilizatorul nu își împărtășește parola cu aplicația terță
- Se bazează pe token-uri de acces cu timp limitat

**Exemplu practic:** Când te loghezi pe un site cu "Conectează-te cu Google":

1. Site-ul te redirecționează către Google
2. Te autentifici la Google (nu la site-ul respectiv)
3. Google întreabă dacă permiți site-ului să acceseze anumite informații
4. După aprobare, Google trimite un token către site
5. Site-ul folosește token-ul pentru a accesa informațiile tale de la Google

**Componente principale OAuth2:**

- **Resource Owner** - utilizatorul care deține datele
- **Client** - aplicația care vrea acces la date
- **Authorization Server** - serverul care emite token-uri (ex: Google)
- **Resource Server** - serverul care găzduiește datele protejate

### **JWT (JSON Web Token)**

JWT este un **standard pentru token-uri** care conține informații în format JSON, codat și semnat digital.

**Structura unui JWT:**

```
header.payload.signature
```

Exemplu decodat:

- **Header**: informații despre tip și algoritm de criptare
- **Payload**: datele efective (claims) - ID utilizator, permisiuni, expirare
- **Signature**: semnătura digitală pentru verificarea autenticității

**Avantaje JWT:**

- **Stateless** - serverul nu trebuie să stocheze sesiuni
- **Portabil** - poate fi transmis între diferite servicii
- **Securizat** - semnat digital, poate fi și criptat
- **Self-contained** - conține toate informațiile necesare

### **OAuth2 + JWT împreună**

OAuth2 definește **fluxul** de autorizare, iar JWT este **formatul** token-ului folosit.

**Flux tipic:**

1. Utilizatorul se autentifică prin OAuth2
2. Serverul de autorizare emite un **Access Token** în format JWT
3. Clientul include JWT-ul în header-ul cererilor către API
4. API-ul validează JWT-ul și extrage informațiile despre utilizator

**Exemplu de utilizare în header:**

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### **Cazuri de utilizare practice**

- **Single Sign-On (SSO)** - o singură autentificare pentru multiple aplicații
- **API-uri RESTful** - autorizare pentru servicii web
- **Microservicii** - comunicare securizată între servicii
- **Aplicații mobile** - token-uri pentru sesiuni persistente
- **Integrări third-party** - acces controlat la resurse

Această combinație oferă un sistem robust de securitate, scalabil și ușor de implementat pentru aplicații moderne.