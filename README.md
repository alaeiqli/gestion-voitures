# 🚗 Application de Gestion de Voitures - Fullstack

Ce dépôt unique regroupe l'ensemble de l'application de gestion de voitures, divisée en un frontend développé avec **React** et un backend avec **Spring Boot**.
Grâce à l'orchestration avec **Docker Compose**, l'intégralité de l'architecture (y compris la base de données **MariaDB**) est configurée pour s'exécuter en une seule ligne de commande, sans aucune installation manuelle requise.

## 🛠️ Structure de l'Architecture

* **`/myapp`** : Interface utilisateur (Frontend) développée en React.js, servie par un serveur Nginx.
* **`/SpringDataRest`** : API REST (Backend) développée avec Spring Boot (Java 17).
* **`docker-compose.yml`** : Fichier d'orchestration qui interconnecte les conteneurs et gère la base de données MariaDB.

---

## 🚀 Guide d'Exécution de l'Application (Pour l'Enseignant)

Pour déployer et tester l'application sur votre machine, suivez ces quelques étapes :

### 1. Prérequis

Assurez-vous que l'application [Docker Desktop](https://www.docker.com/products/docker-desktop/) est installée et démarrée sur votre machine hôte.

### 2. Cloner le projet unique

Ouvrez votre terminal et clonez ce dépôt :

```bash
git clone https://github.com/alaeiqli/gestion-voitures.git
cd gestion-voitures
```

### 3. Lancer l'environnement complet

Exécutez la commande suivante à la racine du projet :

```bash
docker-compose up --build -d
```

> **Note :** Cette commande va automatiquement télécharger l'image MariaDB, compiler le code source Java, packager le build React dans Nginx et lier l'ensemble des conteneurs sur un réseau virtuel isolé.

---

## 🎯 Accès aux Services de l'Application

Une fois que les conteneurs affichent un statut actif, vous pouvez ouvrir et utiliser l'application :

| Service | URL |
|---|---|
| 💻 **Interface Utilisateur (React)** | [http://localhost:3000](http://localhost:3000) |
| 🚗 **API REST (Spring Boot)** | [http://localhost:8081/api](http://localhost:8081/api) |

---

## 🛑 Arrêter et nettoyer l'infrastructure

Pour stopper l'ensemble des conteneurs et libérer les ports proprement sur votre machine, exécutez simplement :

```bash
docker-compose down
```

---


