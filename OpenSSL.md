
---

## 📌 Définition simple

**OpenSSL** est une bibliothèque logicielle et une suite d'outils en ligne de commande utilisés pour implémenter les protocoles de sécurité **[[TLS]]** et **[[SSL]]**. 

C'est l'outil standard de l'industrie. La quasi-totalité des serveurs web (Nginx, Apache), des clients (Navigateurs) et des outils de développement utilisent OpenSSL en arrière-plan pour gérer le chiffrement, les certificats et les clés.

## 💡 Pourquoi est-ce utile pour un développeur ?

En tant que développeur, tu utiliseras OpenSSL pour :

*   Générer tes propres clés et certificats pour tes environnements de test (local).
*   Vérifier si un serveur distant possède un certificat valide.
*   Déboguer des problèmes de connexion TLS.
*   Chiffrer ou déchiffrer des fichiers localement.

## 🚀 Commandes courantes et utiles

*Note : Ces commandes sont les standards sur Ubuntu. Elles utilisent souvent la génération de clés RSA 2048 bits (le standard actuel).*

### 1. Créer une paire de clés ([[Clé Privée|Privée]] et [[Clé Publique|Public]])
Pour créer une clé privée (le fichier `.key`) :
```bash
openssl genrsa -out mon_serveur.key 2048
```

### 2. Générer une [[CSR]] (Demande de Certificat)
Pour créer le fichier de demande en utilisant la clé privée précédente :
```bash
openssl req -new -key mon_serveur.key -out mon_serveur.csr
```
*(On te posera des questions ici : pays, nom de l'organisation, nom de domaine, etc.)*

### 3. Créer un [[Certificat auto-signé]] (Idéal pour le développement local)
Si tu veux tester ton site en `https://localhost` sans passer par une autorité de certification officielle, tu peux créer ton propre certificat valide pour une durée de 365 jours :
```bash
openssl req -x509 -new -key mon_serveur.key -out mon_serveur.crt -days 365
```
*C'est la commande magique pour avoir du HTTPS sur ton PC de dev sans payer ni configurer de CA.*

### 4. Vérifier un [[certificat]] à distance (Debug)
Si tu veux voir les détails du certificat d'un site web (comme Google) pour vérifier sa chaîne de confiance :
```bash
openssl s_client -connect google.com:443 -showcerts
```
*C'est très utile pour voir si un serveur utilise bien le bon protocole ou pour voir qui a signé son certificat.*

### 5. Convertir un [[certificat]] (Exemple : pour le format PKCS12)
Parfois, un logiciel te demande un fichier `.p12` ou `.pfx` au lieu d'un `.crt`. OpenSSL permet de convertir :
```bash
openssl pkcs12 -export -out certificat.p12 -inkey mon_serveur.key -in mon_serveur.crt
```

## 📝 Résumé

*   **OpenSSL** = La bibliothèque standard pour la sécurité sur le web.
*   **Usage Dev** = Générer des clés, créer des certificats pour le local, tester les connexions.
*   **Fichiers clés** : 
    *   `.key` : Ta clé privée (à garder secret).
    *   `.csr` : Ta demande de certificat (à envoyer à la CA).
    *   `.crt` ou `.pem` : Ton certificat public (à donner au monde).

