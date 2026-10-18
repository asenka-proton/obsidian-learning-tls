
- - - 

## 🎯 Objectifs de l'exercice

1. **Pratique** : Apprendre à générer des clés et des certificats avec [[OpenSSL]].
2. **Comparaison** : Visualiser la différence concrète entre une communication en clair (HTTP) et une communication sécurisée ([[HTTPS]]).

---

## 🛠 Étape 1 : Le monde "Sans TLS" (HTTP)

L'objectif est de voir comment les données circulent sans protection.

**Action :** Créer un serveur web ultra-léger en Python.

1. Créer un fichier `server_http.py` :

   ```python
   from http.server import HTTPServer, BaseHTTPRequestHandler

   PORT = 8080

   class MyHandler(BaseHTTPRequestHandler):
       def do_GET(self):
           self.send_response(200)
           self.send_header("Content-type", "text/html")
           self.end_headers()
           # Donnée sensible !
           self.wfile.write(b"SECRET_DATA: L'utilisateur est 'Asenka' et son IP est 127.0.0.1")

   with HTTPServer(("", PORT), MyHandler) as httpd:
       print(f"Serveur HTTP démarré sur le port {PORT}...")
       httpd.serve_forever()
   ```
   
2. Lancer le serveur : `python3 server_http.py`

3. Tester avec `curl http://127.0.0.1:8080`

> **Analyse :** Le message est lisible par n'importe qui sur le réseau. C'est le protocole **HTTP** pur : aucun chiffrement, aucune authentification, aucune intégrité.

---

## 🔐 Étape 2 : La "Fábrique à Certificats" (OpenSSL)

Ici, nous créons les briques de confiance. Nous utilisons la cryptographie asymétrique pour établir une identité.

**Action :** Générer une paire de clés et un [[certificat auto-signé]].

1. **Générer la [[Clé Privée]] (`server.key`) :**
   *C'est notre secret absolu. Il doit rester dans notre [[Key Store]].*
   ```bash
   openssl genrsa -out server.key 2048
   ```
   
2. **Générer la [[CSR]] (`server.csr`) :**
   *C'est notre formulaire de demande d'identité. On y met notre [[Clé Publique]].*
   ```bash
   openssl req -new -key server.key -out server.csr
   ```
   
3. **Générer le [[Certificat Auto-signé]] (`server.crt`) :**
   *Ici, nous jouons le rôle de la [[CA]] (Autorité de Certification) en [[Signature|signant]] notre propre demande.*
   ```bash
   openssl x509 -req -in server.csr -key server.key -out server.crt -days 365
   ```


---

## 🛡️ Étape 3 : Le "Tunnel Sécurisé" ([[HTTPS]])

Nous mettons en place le protocole [[TLS]] pour sécuriser le transport.

**Action :** Modifier le serveur pour utiliser le protocole [[TLS]].

1. Créer le fichier `server_ssl.py` :

   ```python
   from http.server import HTTPServer, BaseHTTPRequestHandler
   import ssl

   PORT = 8443

   class MyHandler(BaseHTTPRequestHandler):
       def do_GET(self):
           self.send_response(200)
           self.send_header("Content-type", "text/html")
           self.end_headers()
           self.wfile.write(b"SECRET_DATA: L'utilisateur est 'Asenka' et son IP est 127.0.0.1")

   # Configuration SSL : On charge notre Key Store et notre certificat
   context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
   context.load_cert_chain(certfile="server.crt", keyfile="server.key")

   httpd = HTTPServer(("", PORT), MyHandler)
   # Enveloppement du socket avec le protocole TLS
   httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

   print(f"Serveur HTTPS démarré sur le port {PORT}...")
   httpd.serve_forever()
   ```
   
2. Lancer le serveur : `python3 server_ssl.py`

3. Tester avec `curl --insecure https://127.0.0.1:8443`
   *(L'option `-k / --insecure` est nécessaire car notre certificat n'est pas dans le **[[Trust Store]]** du système).*

---

## 📊 Synthèse et Analyse Finale

| Concept                 | Observation dans l'exercice                                                | Rôle technique                                      |
| :---------------------- | :------------------------------------------------------------------------- | :-------------------------------------------------- |
| **Clé Privée (`.key`)** | Utilisée par le serveur pour déchiffrer les données.                       | **Key Store** : Identité secrète.                   |
| **Certificat (`.crt`)** | Envoyé au client pour prouver l'identité.                                  | **Identité Publique** : Utilisée pour le Handshake. |
| **Handshake**           | Le client et le serveur s'accordent sur une clé de session.                | **ECDHE** : Échange de clés avec **PFS**.           |
| **Trust Store**         | Le `curl` affiche une alerte car il ne connaît pas ma CA.                  | **Confiance** : Liste des notaires reconnus.        |
| **Cipher Suite**        | Le serveur choisit la "recette" de chiffrement.                            | **Performance** : Équilibre sécurité/vitesse.       |
| **PFS**                 | Même si `server.key` est volée plus tard, les sessions passées sont sûres. | **Sécurité** : Clés de session éphémères.           |
|                         |                                                                            |                                                     |

### 💡 Conclusion pour le coffre

L'expérience montre que le [[TLS]] n'est pas juste "un cadenas". C'est une négociation complexe en plusieurs étapes :
1. **Authentification** : Qui es-tu ? (Certificats / CA / TrustStore).
2. **Négociation** : Comment allons-nous nous parler ? (Cipher Suites / ECDHE).
3. **Chiffrement** : On parle maintenant en secret (Clés de session symétriques).
4. **Intégrité** : On vérifie que personne n'a modifié le message (HMAC / Hachage).