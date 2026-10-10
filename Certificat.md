_Certificat Numérique (Digital Certificate)_

---

## 📌 Définition simple

Un **certificat numérique** est une **carte d'identité numérique**. 
C'est un fichier électronique qui lie une **identité** (un nom de domaine comme `google.com` ou une personne) à une **[[clé publique]]** spécifique. 

Il sert à prouver que "la [[clé publique]] que tu vois appartient bien au propriétaire que tu penses être".

## 🛂 L'analogie du Passeport

Pour comprendre un certificat, imagine un **passeport physique** :
1.  **L'identité :** Ton nom, ta photo, ta date de naissance (C'est le nom du site et sa clé publique).
2.  **La signature :** Le tampon officiel du gouvernement (C'est la signature de l'Autorité de Certification).
3.  **La confiance :** Tu fais confiance au passeport parce que tu fais confiance au gouvernement qui l'a délivré.

**Sans certificat :** Quelqu'un pourrait te montrer une carte d'identité faite maison. Tu as la clé publique, mais tu n'as aucune preuve qu'elle appartient à la vraie banque.
**Avec certificat :** Le gouvernement (une autorité de confiance) a "tamponné" le document pour dire : *"Je certifie que cette clé publique appartient bien à cette banque"*.



## 📦 Que contient un certificat ? 

Même si c'est un fichier complexe, un certificat contient principalement :
*   **Le Sujet :** Le nom du propriétaire (ex: `www.tonsite.com`).
*   **La [[Clé Publique]] :** La clé utilisée pour chiffrer les données.
*   **L'Émetteur :** Qui a délivré le certificat (ex: Let's Encrypt, DigiCert).
*   **La Date d'expiration :** Jusqu'à quand le certificat est valide.
*   **La [[Signature]] Numérique :** La preuve cryptographique que l'émetteur a bien validé ce certificat.

## 🔗 La Chaîne de Confiance (Chain of Trust)

C'est ici que la magie opère. Comment ton navigateur sait-il que le certificat de `google.com` est vrai ? Il utilise une **chaîne**.

1.  **Certificat de Site :** Signé par une autorité intermédiaire.
2.  **Certificat Intermédiaire :** Signé par une Autorité de Certification (CA) racine.
3.  **[[CA|CA]] Racine (Root CA) :** C'est le sommet de la pyramide.

**Le secret :** Ton système d'exploitation (Ubuntu) et ton navigateur possèdent déjà une liste de "CA Racines" de confiance pré-installées. Si la chaîne remonte jusqu'à l'une de ces racines, le site est considéré comme sûr.

### Schéma de la Chaîne de Confiance
```mermaid
graph TD
    A[CA Racine - 'Le Gouvernement'] -->|Signe| B[CA Intermédiaire - 'La Préfecture']
    B -->|Signe| C[Certificat du Site - 'Ton Passeport']
    
    style A fill:#f96,stroke:#333,stroke-width:2px
    style B fill:#6cf,stroke:#333,stroke-width:2px
    style C fill:#9f9,stroke:#333,stroke-width:2px
```

## 🔄 Le flux de vérification 

Voici ce qui se passe quand tu tapes `https://tonsite.com` (en [[TLS]] simple):

```mermaid
sequenceDiagram
    autonumber
    participant C as 💻 Navigateur (Client)
    participant S as 🖥️ Serveur Web
    
    Note over C, S: Étape 1 : La demande de connexion
    C->>S: "Salut ! Je veux accéder à ton site."
    S-->>C: "Ok, voici mon Certificat (Identité + Clé Publique)"

    Note over C: Étape 2 : Le "Check" de sécurité interne
    
    rect rgb(240, 240, 240)
        Note right of C: 🔍 ANALYSE DU CERTIFICAT :
        Note right of C: 1. Signature : Est-ce que ce certificat<br/>a été signé par une autorité que je connais ?
        Note right of C: 2. Validité : Est-ce qu'il est encore<br/>à jour (date d'expiration) ?
        Note right of C: 3. Domaine : Est-ce que le nom<br/>sur le certificat correspond bien à l'URL ?
    end

    alt ✅ Certificat Valide
        C->>S: "Ok, je te fais confiance. On peut parler en privé."
        Note over C,S: Établissement du tunnel TLS (Chiffrement)
    else ❌ Certificat Invalide
        C->>C: Affiche l'alerte rouge :<br/>"Votre connexion n'est pas privée"
        Note over C: Bloque l'accès au site
    end
```


## 📝 Résumé 

*   **Certificat ≠ Clé Privée :** Le certificat contient la clé publique. La clé privée reste secrète sur le serveur.
*   **Certificat ≠ Identité seule :** Un certificat sans signature d'une [[CA|autorité de confiance]] n'a aucune valeur de sécurité.
*   **CA ([[CA]]) :** Ce sont les "notaires" d'Internet. Ils vérifient l'identité des gens et signent leurs certificats.
