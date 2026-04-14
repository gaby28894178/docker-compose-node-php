
# docker-compose-node-php

Proyecto base con Docker Compose para levantar:

- **MySQL 8** compartido
- **PHP 8.2 (php-fpm) + Nginx**
- **Node.js 20**
- **Adminer** para administrar la BD desde el navegador

## Servicios y puertos

- **MySQL**: `localhost:3307` (mapea al `3306` del contenedor)
- **PHP + Nginx**: `http://localhost:8080`
- **Node**: `http://localhost:3000`
- **Adminer**: `http://localhost:8081`

## Estructura del proyecto

- **`docker-compose.yml`**: definición de servicios
- **`src_php/`**: código PHP
  - `index.php` (actualmente vacío)
  - `db.php` (actualmente vacío)
- **`src_node/`**: código Node
  - `index.js` (actualmente vacío)

## Requisitos

- Docker Desktop (o Docker Engine) con soporte para Docker Compose

## Cómo levantar el entorno

1. Desde la raíz del proyecto, ejecutá:

```bash
docker compose up -d
```

2. Verificá los contenedores:

```bash
docker compose ps
```

3. Accesos:

- PHP (Nginx): `http://localhost:8080`
- Node: `http://localhost:3000`
- Adminer: `http://localhost:8081`

## Base de datos (MySQL)

La base se crea automáticamente con estos parámetros (definidos en `docker-compose.yml`):

- **Host (desde contenedores)**: `db`
- **Host (desde tu PC)**: `127.0.0.1`
- **Puerto (desde tu PC)**: `3307`
- **Database**: `proyect1_db`
- **User**: `root`
- **Password**: `root`

Persistencia:

- Se guarda en `./mysql_data` (volumen montado a `/var/lib/mysql`).

## Adminer

Entrá en `http://localhost:8081` y usá, por ejemplo:

- **System**: MySQL
- **Server**: `db`
- **Username**: `root`
- **Password**: `root`
- **Database**: `proyect1_db`

## Notas importantes (estado actual del repo)

- **Falta `nginx.conf`**: el servicio `nginx` en `docker-compose.yml` monta `./nginx.conf` pero ese archivo no está en el repo. Mientras no exista, el contenedor de Nginx va a fallar al iniciar.
- **Código de Node/PHP vacío**: `src_node/index.js`, `src_php/index.php` y `src_php/db.php` están vacíos, así que por ahora no hay endpoints/páginas definidos.
- **Node instala dependencias al iniciar**: el contenedor `node-app` corre `npm install && npm start`. Para que funcione bien, normalmente necesitás un `package.json` dentro de `src_node/` (en el repo actual no existe).

## Troubleshooting

- **Nginx no levanta / error de mount de `nginx.conf`**
  - Creá el archivo `nginx.conf` en la raíz del proyecto, o ajustá el `docker-compose.yml` para no montarlo.

- **Node no levanta (no hay `package.json`)**
  - Agregá un `package.json` en `src_node/` con un script `start`, o cambiá el `command` del servicio `node-app`.

- **Conflicto de puertos**
  - Si ya usás `3000`, `8080`, `8081` o `3307` en tu PC, cambiá los mapeos en `docker-compose.yml`.
