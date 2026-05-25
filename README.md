# 🚗 Application de Gestion de Voitures - Fullstack - Réalisée par: ALAE IQLI / Filière: D2S

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
---

## ☸️ Guide de Déploiement avec Kubernetes (Minikube)

Le projet intègre une configuration complète pour simuler un déploiement cloud résilient en utilisant **Kubernetes**. Le backend Spring Boot est configuré pour s'exécuter sur un cluster répliqué avec basculement automatique vers la base de données.

### 1. Prérequis Kubernetes
* **Minikube** installé et démarré (`minikube start`).
* **kubectl** configuré pour interagir avec le cluster.

### 2. Préparer l'environnement Docker de Minikube
Pour que Minikube puisse lire directement les images Docker construites localement sans passer par Docker Hub, configurez votre terminal pour utiliser le démon Docker interne du cluster :

Sur Windows (CMD) :
```cmd
@FOR /f "tokens=*" %i IN ('minikube -p minikube docker-env --shell cmd') DO @%i

##3. Compiler le projet et construire l'image du Backend
Naviguez dans le dossier backend pour packager l'application (le code a été nettoyé de Lombok pour garantir une compilation standard) et créez l'image Docker :

Bash
cd SpringDataRest
mvn clean package -DskipTests
docker build -t springboot-carpooling-k8s:1.0 .
cd ..
4. Appliquer les manifestes Kubernetes
Déployez d'abord la base de données MariaDB, puis le backend répliqué :

Bash
# Lancement de la base de données MariaDB (Service & Deployment)
kubectl apply -f db-deployment.yaml

#Lancement de l'API Spring Boot (Service & Deployment à 3 répliques)
kubectl apply -f app-deployment.yaml
##5. Vérifier l'état de l'infrastructure
Pour vous assurer que la réplication de l'API et la persistance fonctionnent :

Bash
kubectl get pods
Vous observerez l'architecture suivante s'exécuter de manière stable :

carpooling-db-xxx (1/1 READY) : Le conteneur MariaDB centralisé.

carpooling-backend-deployment-xxx (3 Pods distincts) : Assurant la haute disponibilité du backend (stratégie Hibernate configurée sur update pour éviter les collisions d'initialisation).

##6. Accéder à l'API sur Kubernetes
Pour exposer le point d'accès réseau et obtenir l'URL de connexion sur votre machine hôte, lancez le tunnel Minikube :

Bash
minikube service carpooling-backend-svc --url
Laissez ce terminal ouvert et utilisez l'adresse IP/Port fournie (ex: http://127.0.0.1:xxxxx/api) dans votre navigateur ou sur Postman pour interagir avec les endpoints REST.

##🛑 Arrêter et nettoyer l'infrastructure Kubernetes
Bash
kubectl delete -f app-deployment.yaml
kubectl delete -f db-deployment.yaml


