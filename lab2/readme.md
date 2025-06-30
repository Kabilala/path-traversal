 
# 🧪 Lab 2 - Path Traversal: Bypass de Filtrage

---

## 🔍 Description du Lab

Ce lab contient une vulnérabilité **Path Traversal** dans la fonctionnalité d'affichage des images produits.  
Bien que l'application bloque les séquences classiques de traversée (`../`), elle considère le **nom du fichier fourni comme relatif à un répertoire de travail par défaut**.

---

## 🎯 Objectif

Exploiter la vulnérabilité pour accéder au fichier sensible `/etc/passwd`.

---

## 🛠️ Outils utilisés

- 🔧 **Burp Suite Community ou Pro**
- 🌐 Navigateur Web
- 🧠 Connaissances de base sur les chemins UNIX et les encodages URL

---

## 🚦 Étapes de l’exploitation

### 1. Intercepter une requête d'image

- Naviguer sur un produit affichant une image.
- Utiliser **Burp Suite** pour intercepter la requête GET vers `/image?filename=...`.

### 2. Modifier le paramètre `filename`

- Remplacer sa valeur par un chemin absolu :
```

/etc/passwd

```

### 3. Valider la requête modifiée

- L’application ne bloque pas ce chemin car il ne contient **aucune séquence de traversée (`../`)**.
- Elle lit directement depuis le répertoire de travail et accède au fichier système.

### 4. Vérifier le contenu de la réponse

- Tu dois voir un contenu de type :
```

root\:x:0:0\:root:/root:/bin/bash
daemon\:x:1:1\:daemon:/usr/sbin:/usr/sbin/nologin
...

```

📌 **💡 Astuce** : La validation du lab est automatique dès que le contenu de `/etc/passwd` est lu par le serveur.

---

## 📸 Capture d’écran 

*Ajoute ici une capture Burp Suite montrant la requête modifiée et la réponse contenant `/etc/passwd`.*

![path-traversale](https://github.com/Kabilala/path-traversal/blob/main/lab2/lab2.png)

---

## 🛡️ Leçons à retenir

- Bloquer uniquement les séquences `../` ne suffit **pas**.
- Une application reste vulnérable si elle accepte des **chemins absolus non validés**.
- Il faut toujours **restreindre et valider** la valeur des paramètres utilisateur côté serveur.
- L’accès aux fichiers doit se faire uniquement via une **liste blanche** de fichiers autorisés.

---

## 🧠 Bonus Technique

- Filtrage contourné ici grâce à l’utilisation directe d’un chemin absolu (`/etc/passwd`).
- Aucune traversée (`../`) n’a été nécessaire, ce qui rend l’exploitation **plus subtile**.

---
