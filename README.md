# Projet Etudiants

Un petit projet d'exemple composé d'une API Spring Boot (Java) et d'une application mobile Expo (React Native) pour gérer une liste d'étudiants. La base de données utilisée est PostgreSQL, fournie via Docker Compose.

## Structure

- `docker-compose.yml` : compose pour PostgreSQL et le service API.
- `api-spring-boot/` : service backend Spring Boot (Java 21, Maven wrapper inclus).
- `mobile_app/` : application mobile basée sur Expo (React Native).

## Pré-requis

- Java 21
- Maven (ou utiliser le wrapper `mvnw` / `mvnw.cmd` fourni)
- Docker et Docker Compose
- Node.js et `npm` (pour l'app mobile)
- Expo CLI (optionnel pour le développement mobile, `npm install -g expo-cli`)

## Analyse rapide du projet

- L'API est dans `api-spring-boot` et expose un endpoint `GET /api/etudiants` implémenté dans `EtudiantController`.
- Le modèle `Etudiant` se trouve dans `api-spring-boot/src/main/java/com/example/apispringboot/model/Etudiant.java` et utilise JPA + Lombok.
- Les données d'exemple sont insérées automatiquement au démarrage par `DataInitializer` (si la table est vide).
- La configuration de la base de données est dans `api-spring-boot/src/main/resources/application.properties` et pointe vers `jdbc:postgresql://db:5432/etudiants_db` (service `db` défini dans `docker-compose.yml`).
- L'application mobile est un projet Expo (fichiers principaux `App.js`, `package.json`) qui utilise `axios` pour communiquer avec l'API.

## Démarrage rapide (avec Docker Compose)

1. Construire et démarrer les services :

```bash
docker-compose up --build
```

2. L'API sera accessible sur `http://localhost:8080`.
3. Endpoint principal : `GET /api/etudiants` (exemple) :

```bash
curl http://localhost:8080/api/etudiants
```

Remarques :
- Le conteneur `db` expose PostgreSQL sur le port `5432`.
- Le `DataInitializer` insère plusieurs étudiants d'exemple si la base est vide.

## Exécuter l'API localement (sans Docker)

1. Aller dans le dossier backend :

```bash
cd api-spring-boot
```

2. Sous Windows :

```powershell
mvnw.cmd spring-boot:run
```

Sous macOS / Linux :

```bash
./mvnw spring-boot:run
```

3. L'application se connectera à la base de données configurée dans `application.properties`. Pour exécuter localement sans Docker, adaptez la `spring.datasource.url` vers une instance PostgreSQL locale ou démarrez PostgreSQL séparément.

## Application mobile (développement)

1. Se placer dans le dossier mobile :

```bash
cd mobile_app
npm install
npm start
# ou
npm run android
```

2. En développement avec Expo, iOS/Android peuvent se connecter via tunnel ou sur l'émulateur. Pour que l'application mobile accède à l'API backend sur la machine hôte, utilisez l'URL réseau de la machine (par exemple `http://192.168.x.y:8080`) ou activez le tunnel Expo.

## Endpoints connus

- `GET /api/etudiants` — retourne la liste des étudiants (implémenté).

## Points d'amélioration / notes

- Ajouter des endpoints CRUD complets (POST, PUT, DELETE) pour `Etudiant`.
- Ajouter la validation et gestion des erreurs.
- Ajouter des tests d'intégration et d'API.
- Sécuriser l'API (authentification/autorisation) si nécessaire.

## Liens vers les fichiers importants

- [API entrypoint](api-spring-boot/src/main/java/com/example/apispringboot/ApiSpringBootApplication.java)
- [Controller `EtudiantController`](api-spring-boot/src/main/java/com/example/apispringboot/controller/EtudiantController.java)
- [Modèle `Etudiant`](api-spring-boot/src/main/java/com/example/apispringboot/model/Etudiant.java)
- [Data initializer](api-spring-boot/src/main/java/com/example/apispringboot/config/DataInitializer.java)
- [Configuration BDD](api-spring-boot/src/main/resources/application.properties)
- [Docker Compose](docker-compose.yml)
- [App mobile](mobile_app/package.json)