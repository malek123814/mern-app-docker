# MERN APP

## Vue d'Ensemble
Ce projet est une application full-stack qui consiste en un client React et un serveur Node.js utilisant MongoDB comme base de données. Docker est utilisé pour la conteneurisation, et Docker Compose est utilisé pour orchestrer les services.

## Table des Matières
- [Technologies Utilisées](#technologies-utilisées)
- [Variables d'Environnement](#variables-denvironnement)
- [Configuration de Docker](#configuration-de-docker)
- [Images Docker](#images-docker)
- [Docker Compose](#docker-compose)
- [Comment Exécuter le Projet](#comment-executer-le-projet)

## Technologies Utilisées
- **Frontend** : React
- **Backend** : Node.js, Express
- **Base de Données** : MongoDB
- **Conteneurisation** : Docker, Docker Compose

## Variables d'Environnement
Les variables d'environnement suivantes sont utilisées dans l'application :

- **REACT_APP_API_URL** : Cette variable contient l'URL de base pour le serveur API. Elle est utilisée dans le client React pour faire des requêtes au serveur.
- **MONGO_URI** : L'URI de connexion à MongoDB utilisée par le serveur pour se connecter à l'instance MongoDB.

## Configuration de Docker
Ce projet comprend des Dockerfiles pour le client et le serveur, qui facilitent la construction et l'exécution des services dans des conteneurs isolés. Les configurations incluent :

- **Client** : Un environnement Node.js pour construire l'application React. Les dépendances sont installées et l'application est construite pour une utilisation en production. Un serveur HTTP simple peut être utilisé pour servir l'application construite.
  
- **Serveur** : Un environnement Node.js qui installe les dépendances nécessaires et configure l'application pour écouter sur un port spécifique.

## Images Docker
Les images Docker créées pour ce projet sont les suivantes :

- **Image du Client** : `node:lts-alpine`
- **Image du Serveur** : `node:lts-alpine`
- **Image de la Base de Données** : `mongo:latest`

Ces images sont spécifiées dans les Dockerfiles respectifs et sont utilisées lors de la construction et du déploiement des services.

## Docker Compose
Docker Compose est utilisé pour gérer les différents services de l'application, y compris le client, le serveur et MongoDB. Les services sont interconnectés, ce qui permet une communication fluide entre le client et le serveur. Le fichier de configuration spécifie les images, les ports exposés, ainsi que les variables d'environnement nécessaires pour chaque service.

## Comment Exécuter le Projet
1. Assurez-vous d'avoir Docker et Docker Compose installés sur votre machine.
2. Clonez ce dépôt sur votre machine locale.
3. Accédez au répertoire du projet dans votre terminal.
4. Construisez et démarrez l'application en utilisant Docker Compose :

   ```bash
   docker-compose up --build
   ```

5. Accédez au client à [http://localhost:3000](http://localhost:3000).

---

## Intégration continue avec Jenkins — TP3

Ce projet intègre un pipeline CI avec Jenkins permettant
d'automatiser la construction, l'analyse de sécurité et
la publication des images Docker du projet MERN.

### Outils utilisés

- Jenkins : automatisation du pipeline CI
- GitHub : gestion du code source
- Docker : construction des images
- Trivy : analyse des vulnérabilités
- Docker Hub : stockage des images Docker

### Configuration du pipeline

Le pipeline est défini dans le fichier `Jenkinsfile`
à la racine du projet.

Il utilise deux identifiants Jenkins :

- `github_ssh` : accès SSH au dépôt GitHub
- `dockerhub` : authentification à Docker Hub

### Étapes du pipeline

1. Checkout : récupération du projet depuis GitHub.
2. Build Server : construction de l'image Docker du serveur.
3. Scan Server : analyse de sécurité avec Trivy.
4. Push Server : publication de l'image sur Docker Hub.
5. Build Client : construction de l'image Docker du client.
6. Scan Client : analyse de sécurité avec Trivy.
7. Push Client : publication de l'image sur Docker Hub.
8. Archivage : sauvegarde des rapports Trivy.
9. Nettoyage : déconnexion Docker Hub et suppression
   des images locales construites.

### Exécution du pipeline

1. Démarrer Jenkins.
2. Ouvrir l'interface Jenkins sur http://localhost:8080.
3. Sélectionner le pipeline `TP3-MERN-Docker-CI`.
4. Cliquer sur « Lancer un build ».
5. Consulter « Console Output » pour suivre l'exécution.

Le pipeline est également configuré pour vérifier
les modifications GitHub toutes les cinq minutes environ.

### Images Docker Hub

Les images sont destinées à être publiées sous les noms :

- `malekbejaoui1/mern-server:<BUILD_NUMBER>`
- `malekbejaoui1/mern-client:<BUILD_NUMBER>`

Le numéro du build Jenkins est utilisé comme tag.

### Rapports de sécurité

Les rapports générés par Trivy sont :

- `trivy_server.txt`
- `trivy_client.txt`

Ils sont archivés dans Jenkins lorsqu'ils sont générés.

### Dépôt GitHub

https://github.com/malek123814/mern-app-docker

### Auteur

Malek Bejaoui

