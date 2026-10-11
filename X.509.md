
---
## 📌 Définition simple

**X.509** est le standard international qui définit le format et la structure des **certificats de clés publiques**. 

Si un certificat est une "carte d'identité", X.509 est la **norme ISO** qui définit exactement quelles informations doivent figurer sur cette carte, dans quel ordre, et comment elles doivent être structurées techniquement.

## 🛂 L'analogie du formulaire administratif

Imagine que tu veuilles créer un passeport pour un pays :
*   Tu peux décider de mettre la photo à gauche ou à droite.
*   Tu peux décider d'écrire le nom en bleu ou en rouge.
*   Mais pour que les autres pays puissent lire ton passeport, tu dois respecter une **norme commune**.

**X.509 est cette norme.** C'est grâce à elle qu'un certificat émis par une autorité de certification en France est parfaitement compréhensible par un navigateur aux États-Unis ou par un serveur en Chine.

## 📦 Anatomie d'un certificat X.509 

Bien que le format soit complexe, un certificat X.509 se décompose en plusieurs champs clés :

1.  **Version :** La version du standard X.509 utilisée (généralement la v3).
2.  **Numéro de série :** Un identifiant unique attribué par la CA pour ce certificat.
3.  **Algorithme de signature :** La méthode mathématique utilisée pour signer le certificat (ex: SHA-256 avec RSA).
4.  **Émetteur (Issuer) :** L'identité de l'autorité qui a signé le certificat (la CA).
5.  **Période de validité :** Les dates de début et de fin (Date d'expiration).
6.  **Sujet (Subject) :** L'identité du propriétaire du certificat (ex: `www.google.com`).
7.  **Clé Publique :** La clé publique du propriétaire (celle utilisée pour chiffrer).
8.  **Extensions :** Des informations supplémentaires importantes (ex: les domaines autorisés).

## 💻 Détails techniques pour le développeur

### 1. ASN.1 et encodage

X.509 utilise une syntaxe de description de données appelée **ASN.1**. 
*   **DER** est le format binaire résultant de cette syntaxe (non lisible par l'humain).
*   **PEM** est le format que tu vois souvent dans tes fichiers `.crt` ou `.pem`. C'est simplement le format binaire DER encodé en base64 et entouré de lignes `-----BEGIN CERTIFICATE-----`.

### 2. Le champ "[[SAN]]" (Subject Alternative Name)

C'est une extension très importante du standard X.509. Elle permet d'associer plusieurs noms à un seul certificat. 
*   *Exemple :* Un certificat peut être valide pour `monsite.com`, `www.monsite.com` et l'adresse IP `192.168.1.10` grâce au champ SAN.

## Résumé

*   **X.509** = Le format standard des certificats.
*   **Rôle** : Garantir que tous les certificats du monde ont la même structure pour être interopérables.
*   **Contenu** : Identité du sujet, identité de l'émetteur, dates, clé publique et extensions.
*   **Format de fichier** : Souvent stocké en format **PEM** (Base64) pour être facile à copier-coller dans des fichiers de configuration.
