# Directus - Install guide

A headless CMS powered by Directus and Supabase, deployed via Docker Compose. This repository contains everything you need to run Directus with Supabase as your primary database and storage drive.

## Description

This project sets up Directus configured to use a Supabase database and Supabase storage driver. Directus provides a modern headless CMS interface, while Supabase offers a scalable PostgreSQL database and object storage.

## Features
- ***Directus***: Latest stable release.
- ***Supabase Integration***:
  - PostgreSQL database client.
  - Supabase storage driver for file uploads.
- ***Docker Compose***: Easy spin-up of Directus service.
- ***Environment-driven configuration***: All credentials and settings via `.env` file.

## Prerequisites
- Docker
- Docker Compose
- A Supabase project with:
  - PostgreSQL database credentials
  - Supabase storage bucket and service role credentials

## Getting Started

1. ***Clone the repository***:
```sh
git clone https://github.com/KevinDeBenedetti/directus.git
cd directus
```

2. ***Create your environment file***: Copy the sample environment variables to `.env`:
```sh
cp .env.example .env
```

3. ***Configure `.env`***: Open `.env` and set the following variables:
```sh
# Directus Admin
ADMIN_EMAIL=your-admin@example.com
ADMIN_PASSWORD=yourStrongPassword
SECRET=yourDirectusSecretKey

# Database (Supabase)
DB_CLIENT=pg
DB_HOST=db.<your-supabase-project>.supabase.co
DB_PORT=5432
DB_DATABASE=postgres
DB_USER=postgres
DB_PASSWORD=<your-db-password>

# Supabase Storage
STORAGE_LOCATIONS='supabase'
STORAGE_SUPABASE_DRIVER=adapter
STORAGE_SUPABASE_SERVICE_ROLE=<your-service-role-key>
STORAGE_SUPABASE_BUCKET=<your-storage-bucket>
STORAGE_SUPABASE_PROJECT_ID=<your-supabase-project-id>

# Websockets
WEBSOCKETS_ENABLED=true
```

4. ***Start services***:
```sh
docker-compose up -d
```

5. ***Access Directus***: Visit `http://localhost:8055` and log in using the `ADMIN_EMAIL` and `ADMIN_PASSWORD` from your `.env`.

## Docker Compose Configuration
```yml
services:
  directus:
    image: directus/directus:11.5.1
    ports:
      - 8055:8055
    env_file: .env
    environment:
      SECRET: ${SECRET}
      ADMIN_EMAIL: ${ADMIN_EMAIL}
      ADMIN_PASSWORD: ${ADMIN_PASSWORD}
      DB_CLIENT: ${DB_CLIENT}
      DB_HOST: ${DB_HOST}
      DB_PORT: ${DB_PORT}
      DB_DATABASE: ${DB_DATABASE}
      DB_USER: ${DB_USER}
      WEBSOCKETS_ENABLED: ${WEBSOCKETS_ENABLED}
      STORAGE_LOCATIONS: ${STORAGE_LOCATIONS}
      STORAGE_SUPABASE_DRIVER: ${STORAGE_SUPABASE_DRIVER}
      STORAGE_SUPABASE_SERVICE_ROLE: ${STORAGE_SUPABASE_SERVICE_ROLE}
      STORAGE_SUPABASE_BUCKET: ${STORAGE_SUPABASE_BUCKET}
      STORAGE_SUPABASE_PROJECT_ID: ${STORAGE_SUPABASE_PROJECT_ID}
```

## Environment Variables

| Variable                        | Description                                           |
|---------------------------------|-------------------------------------------------------|
| `ADMIN_EMAIL`                   | Admin user email for Directus                         |
| `ADMIN_PASSWORD`                | Admin user password for Directus                      |
| `SECRET`                        | JWT secret for Directus                               |
| `DB_CLIENT`                     | Database client (e.g., `pg`)                          |
| `DB_HOST`                       | Hostname of Supabase database                         |
| `DB_PORT`                       | Port for Supabase database (default `5432`)           |
| `DB_DATABASE`                   | Database name (usually `postgres`)                    |
| `DB_USER`                       | Database user (usually `postgres`)                    |
| `DB_PASSWORD`                   | Password for the database user                        |
| `STORAGE_LOCATIONS`             | Name of the storage location (e.g., `supabase`)       |
| `STORAGE_SUPABASE_DRIVER`       | Supabase storage driver identifier                    |
| `STORAGE_SUPABASE_SERVICE_ROLE` | Supabase service role key for storage                 |
| `STORAGE_SUPABASE_BUCKET`       | Supabase storage bucket name                          |
| `STORAGE_SUPABASE_PROJECT_ID`   | Supabase project identifier                           |
| `WEBSOCKETS_ENABLED`            | Enable or disable websockets (`true` or `false`)      |

## Usage

- After starting the container, log in to Directus at `http://localhost:8055`.

- Use the Directus admin interface to manage collections, fields, and roles.

- Uploaded files will be stored in your Supabase Storage bucket.