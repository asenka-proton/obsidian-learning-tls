- - -
## Le concept : Le cadenas et la clé

Imagine que tu veux que n'importe qui puisse t'envoyer un message secret, mais que toi seul puisses le lire.

- **La [[Clé Publique]] (Le Cadenas ouvert) :** Tu la distribues à tout le monde. N'importe qui peut l'utiliser pour "fermer" un message.
- **La [[Clé Privée]] (La Clé physique) :** Tu la gardes précieusement pour toi. C'est la seule qui peut "ouvrir" le cadenas.

**Règle d'or :** Ce qui est verrouillé par la [[Clé Publique]] ne peut être déverrouillé _que_ par la [[Clé Privée]] correspondante.

Pour bien comprendre la cryptographie asymétrique, il faut distinguer deux usages majeurs que [[TLS]] utilise :

1. **Le [[Chiffrement]]** (Pour que personne ne puisse lire le message).
2. **La [[Signature]]** (Pour prouver qui a envoyé le message et qu'il n'a pas été modifié).

| Action              | Clé utilisée par l'expéditeur | Clé utilisée par le destinataire | But                          |
| ------------------- | ----------------------------- | -------------------------------- | ---------------------------- |
| **[[Chiffrement]]** | Clé Publique du destinataire  | Clé Privée du destinataire       | **Confidentialité** (Secret) |
| **[[Signature]]**   | Clé Privée de l'expéditeur    | Clé Publique de l'expéditeur     | **Authenticité** (Identité)  |


