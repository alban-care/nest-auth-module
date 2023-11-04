# NEST-AUTH MODULE

Construire le module d'authentification d'une API RESTful sécurisée avec NestJS, prisma, JWT et postgreSQL (docker).

## Initialisation du projet

```bash
$ nest new nest-auth
```

<!-- ## Installation des dépendances et des dépendances de développement

```bash
$ pnpm i @nestjs/jwt @nestjs/passport passport @nestjs/jwt passport-jwt @nestjs/typeorm bcryptjs pg prisma

$ pnpm i -D @types/passport-jwt @types/bcryptjs

``` -->

## Création et initialisation de la base de données

### Installation de PostgreSQL

- En ligne de commande :

```bash
$ docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres
```

- Avec un fichier `docker-compose.yml` et un fichier d'environnement `.env` ::

```bash
# .env
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=nest-auth
```

```bash
# .env.example
POSTGRES_HOST=
POSTGRES_PORT=
POSTGRES_USER=
POSTGRES_PASSWORD=
POSTGRES_DB=
```

```bash
# .gitignore
# (...)

# PostgreSQL
postgres-data/

# env
.env
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:latest
    container_name: postgres
    restart: always
    ports:
      - ${POSTGRES_PORT}:5432
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
```

```bash
$ docker compose up -d
```

### Initialisation de la base de données avec Prisma

> Installation de l'extension VSCODE `Prisma` : <https://marketplace.visualstudio.com/items?itemName=Prisma.prisma>

```bash
$ pnpm i -D prisma @prisma/client
$ npx prisma init
```

- Mise à jour du fichier d'envrionnement `.env` et `.env.example` :

```bash
# .env et .env.example
# (...)

DATABASE_URL="postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${POSTGRES_HOST}:${POSTGRES_PORT}/${POSTGRES_DB}?schema=public"
```

### Ajout du modèle `User` au schéma de la base de données et génération de la migration

````bash

```prisma
// schema.prisma
// (...)
model User {
  id        Int      @id @default(autoincrement())
  username  String   @db.VarChar(65)
  email     String   @unique @db.VarChar(255)
  password  String   @db.VarChar(255)
  role      String[] @default(["ROLE_USER"])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
````

```bash
$ npx prisma migrate dev --name user-model
```

> Vérifier que la migration a bien été effectuée dans le dossier `prisma/migrations`
> Visualiser la base de données dans le navigateur grâce à prismastudio avec la commande `npx prisma studio`

## Documentation :

### NestJS

- [NestJS](https://docs.nestjs.com/)

### Autres

- [PostgreSQL](https://www.postgresql.org/docs/)
- [Prisma](https://www.prisma.io/docs/)
