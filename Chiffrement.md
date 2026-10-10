_On parle ici du chiffrement asymétrique_

_Objectif : "Je veux que seul Bob puisse lire ce que j'envoie."_
- - - 

Ici, on utilise la **[[Clé Publique]] du destinataire** (ici Bob).


```mermaid
sequenceDiagram
    participant Alice
    participant Bob
    
    Note over Bob: Génère sa paire de clés<br/>(Privée & Publique)
    
    Bob->>Alice: Envoie sa Clé Publique (ouverte à tous)
    
    Note over Alice: Chiffre le message avec<br/>la Clé Publique de Bob
    
    Alice->>Bob: Envoie le message chiffré
    
    Note over Bob: Déchiffre le message avec<br/>sa Clé Privée (qu'il garde seul)

```

**Ce qu'il faut retenir :** Si Alice veut envoyer un secret à Bob, elle doit "verrouiller" le message avec le cadenas de Bob ([[Clé Publique]]). Seul Bob possède la clé pour ouvrir ce cadenas ([[Clé Privée]]).