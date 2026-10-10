_Self-signed Certificate_

---
## 📌 Définition simple

Un **certificat auto-signé** est un [[certificat]] numérique dont l'émetteur est la même personne que le sujet. 

En d'autres termes, au lieu de demander à une Autorité de Certification ([[CA]]) de "tamponner" ton certificat, tu le "tamponnes" toi-même avec ta propre clé privée.

## 🛂 L'analogie de la carte d'identité "faite maison"

Imagine que tu as besoin d'une carte d'identité pour entrer dans un bâtiment :

*   **[[Certificat]] CA :** C'est une carte d'identité officielle délivrée par la mairie. Tout le monde reconnaît le tampon de la mairie et te fait confiance.

*   **Certificat Auto-signé :** C'est une carte d'identité que tu as fabriquée toi-même chez toi. Tu y as écrit ton nom et mis ta photo. Techniquement, c'est une carte d'identité, mais comme personne ne reconnaît le "tampon" de ton salon, personne ne te laissera entrer.

## ⚠️ Pourquoi le navigateur affiche-t-il une alerte rouge ?

Ton navigateur (Chrome, Firefox) possède une liste interne de "Notaires de confiance" (le **[[Trust Store]]**). 
Lorsqu'il voit un certificat auto-signé, il fait le chemin de confiance :
1.  Il regarde qui a signé le certificat.
2.  Il voit que c'est le site lui-même.
3.  Il se dit : *"Je ne connais pas ce notaire, donc je ne peux pas garantir que ce site est bien celui qu'il prétend être."*

**Résultat :** Il affiche l'alerte de sécurité pour te prévenir que tu pourrais être en train de parler à un pirate qui se fait passer pour le site.

## 📊 Comparaison : Auto-signé vs CA

| Caractéristique | Certificat Auto-signé | Certificat CA (ex: Let's Encrypt) |
| :--- | :--- | :--- |
| **Coût** | Gratuit | Gratuit (souvent) ou Payant |
| **Rapidité** | Instantané | Quelques minutes |
| **Confiance** | Aucune (Alerte rouge) | Totale (Cadenas vert) |
| **Usage idéal** | Développement local, tests internes | Sites web publics, production |


## Résumé

*   **Auto-signé** = Tu es ton propre notaire.
*   **Avantage** = Gratuit et immédiat pour tes tests de dev.
*   **Inconvénient** = Le navigateur ne fait pas confiance par défaut car il ne connaît pas "ton" autorité.
*   **Règle d'or** : Jamais utiliser de certificats auto-signés en production (sauf cas très spécifiques d'infrastructure privée).

