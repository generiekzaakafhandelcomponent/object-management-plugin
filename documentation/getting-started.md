# Aan de slag

## Vereisten

- Java 21
- Node.js >= 20
- Docker & Docker Compose

## De applicatie draaien

Alle onderstaande commando's voer je uit vanuit de **hoofdmap** van het project.

### 1. Omgeving configureren

Kopieer `.env.properties.example` naar `.env.properties` en vul de vereiste waarden in.

### 2. Docker-afhankelijkheden starten

Zorg dat Docker draait en start vervolgens de benodigde services:

```shell
./gradlew :backend:app:composeUp
```

### 3. De backend starten

```shell
./gradlew :backend:app:bootRun
```

### 4. De frontend starten

```shell
nvm use 20
cd frontend
npm install
npm start
```

### Keycloak-gebruikers

De applicatie heeft een aantal voorgeconfigureerde testgebruikers.

| Naam         | Rol            | Gebruikersnaam | Wachtwoord |
|--------------|----------------|----------------|------------|
| James Vance  | ROLE_USER      | user           | user       |
| Asha Miller  | ROLE_ADMIN     | admin          | admin      |
| Morgan Finch | ROLE_DEVELOPER | developer      | developer  |

## Plugin-ontwikkeling

De broncode van de plugin staat in:
- Backend: `backend/plugin/src/`
- Frontend: `frontend/projects/plugin/src/`

Voor meer informatie over het bouwen van een plugin, zie de documentatie
[Custom Plugin Definition](https://docs.valtimo.nl/features/plugins/plugins/custom-plugin-definition).
