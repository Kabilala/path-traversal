
# Lab Path Traversal Vulnerability

---

## 🕵️‍♀️ Description

Ce lab illustre la vulnérabilité **Path Traversal** (Directory Traversal), où un attaquant peut accéder à des fichiers sensibles du serveur en manipulant les paramètres d'entrée liés aux chemins de fichiers.

---

## 🎯 Objectif

- Comprendre le mécanisme de la vulnérabilité Path Traversal.
- Apprendre à identifier et exploiter cette faille sur une application web vulnérable.
- Proposer des solutions pour la corriger.

---

## ⚙️ Contexte technique

L’application prend en entrée un nom de fichier via un paramètre URL, par exemple :

```

[http://example.com/download?file=report.pdf](http://example.com/download?file=report.pdf)

```

Sans validation correcte, il est possible d’accéder à des fichiers système en injectant des séquences `../` :

```

[http://example.com/download?file=../../../../etc/passwd](http://example.com/download?file=../../../../etc/passwd)

```

---

## 🧪 Étapes d’exploitation

1. Identifier le paramètre vulnérable (ex: `file`).
2. Injecter des payloads de type `../` pour remonter dans l’arborescence.
3. Vérifier l’accès à des fichiers sensibles (`/etc/passwd`, `C:\windows\win.ini`).
4. Utiliser Burp Suite ou un navigateur pour automatiser la recherche.

---

## 📸 Screenshot du Lab

*Insérer ici une capture d'écran démontrant l’exploitation réussie de la vulnérabilité.*

![Path-Traversal](https://github.com/Kabilala/path-traversal/blob/main/lab1/lab1.png)

---

## 🛡️ Contremesures

- Valider et normaliser les entrées utilisateurs.
- Utiliser une liste blanche de fichiers autorisés.
- Restreindre l’accès via sandboxing ou chroot.
- Ne jamais concaténer directement des inputs utilisateurs dans les chemins.

---

## 🔗 Ressources

- [OWASP Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [CWE-22: Improper Limitation of a Pathname to a Restricted Directory](https://cwe.mitre.org/data/definitions/22.html)
- [Burp Suite](https://portswigger.net/burp)
- [TryHackMe Path Traversal Room](https://tryhackme.com/room/pathtraversal)

---

## Auteur

Kaouthar Belkebir | Junior Penetration Tester | Cybersecurity Enthusiast

---

