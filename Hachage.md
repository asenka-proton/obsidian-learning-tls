_Hashing_

---

## 📌 Définition simple

Le **hachage** est une fonction mathématique qui prend une donnée de n'importe quelle taille (un mot, un fichier, une vidéo) et la transforme en une chaîne de caractères de **taille fixe**. 

On appelle ce résultat un **"Hash"** ou une **"Empreinte numérique"**.

## 🍹 L'analogie du Smoothie

Imagine que tu passes des fruits dans un mixeur pour faire un smoothie :
1.  Tu mets des fraises, une banane et du lait (tes **données d'entrée**).
2.  Tu mixes le tout (la **fonction de hachage**).
3.  Tu obtiens un smoothie rouge (le **hash**).

**Propriétés clés du smoothie :**
*   **Irréversibilité :** Même si tu as le smoothie, tu ne peux pas "dé-mixer" les fruits pour les récupérer. C'est une fonction à **sens unique**.
*   **Déterminisme :** Si tu mets exactement les mêmes fruits et la même quantité, tu obtiendras toujours le même goût de smoothie.
*   **Effet d'avalanche :** Si tu ajoutes juste une petite pincée de sel dans tes fruits de départ, le goût du smoothie final sera totalement différent.

## 🔑 Propriétés mathématiques d'un bon Hash

Pour être utile en sécurité, une fonction de hachage doit respecter 4 règles :
1.  **Déterministe :** Même entrée = Même sortie.
2.  **Rapide :** Il doit être facile de calculer le hash.
3.  **Irréversible :** Impossible de retrouver la donnée d'origine à partir du hash.
4.  **Résistance aux collisions :** Il doit être quasiment impossible que deux données différentes produisent le même hash.

## ⚠️ Hachage vs [[Chiffrement]] 

C'est l'erreur la plus courante chez les développeurs.

| Caractéristique | Chiffrement (Encryption) | Hachage (Hashing) |
| :--- | :--- | :--- |
| **Objectif** | Cacher une information. | Vérifier l'intégrité d'une information. |
| **Réversibilité** | **Réversible** (avec une clé, on peut déchiffrer). | **Irréversible** (on ne peut jamais "déchiffrer"). |
| **Usage type** | Envoyer un message secret. | Vérifier qu'un fichier n'a pas été modifié. |
| **Analogie** | Un coffre-fort avec une clé. | Une empreinte digitale ou un scellé de cire. |

## 🛡️ Le rôle du Hachage dans le [[TLS]]

Dans le protocole [[TLS]], le hachage est utilisé pour l'**Intégrité**.

Lorsqu'une donnée est envoyée, le système calcule son hash. Le destinataire recalcule le hash de la donnée reçue.
*   Si les deux hashs sont identiques $\rightarrow$ La donnée n'a pas été modifiée pendant le trajet.
*   Si les deux hashs sont différents $\rightarrow$ Quelqu'un a modifié un seul bit de la donnée, et la connexion est coupée.

*Note : Quand on combine le hachage avec une [[clé secrète ]]pour vérifier l'intégrité, on appelle cela un **[[HMAC]]** (Hash-based Message Authentication Code).*

## 💻 Algorithmes courants

*   **SHA-256 :** Le standard actuel, très sûr et rapide.
*   **SHA-3 :** La génération suivante, encore plus robuste.
*   **MD5 / SHA-1 :** **Obsolètes et dangereux.** Ils sont trop simples et des pirates peuvent créer des "collisions" (deux fichiers différents avec le même hash). Ne jamais les utiliser pour de la sécurité.

## Résumé

*   **Hachage** = Transformation d'une donnée en une "empreinte" fixe et unique.
*   **Sens unique** = On ne peut pas revenir en arrière.
*   **Intégrité** = C'est son rôle principal dans le TLS (vérifier que rien n'a été changé).
*   **Différence clé** = Le hachage n'est pas du chiffrement ; on ne cherche pas à cacher l'information, mais à vérifier sa validité.

