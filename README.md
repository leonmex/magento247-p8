# Sunday PHP Magento Developer Coding Challenge

This project provides a Docker-based development environment for a Magento 2.4.7-p8 codebase.  
The Magento application itself (for example the **magento247-p8** repository) lives in the `code/` folder and is mounted into the containers at `/var/www/html`.

## Services (docker-compose.yml)

The root `docker-compose.yml` defines the following services:

- `db` – MariaDB 10.6 database with persistent volume `magento-db-data`.
- `opensearch` – OpenSearch 2.x instance used as Magento search engine.
- `redis` – Redis 6.2 used for caching/sessions.
- `phpfpm` – PHP 8.2-FPM container with all Magento-required extensions and Composer installed. Mounts `./code` to `/var/www/html`.
- `cron` – PHP 8.2-FPM container running Magento cron.
- `web` – Nginx container serving the Magento storefront and admin, mounting `./code` and `conf/nginx/nginx.conf`.

## Layout

- `code/` – Magento 2.4.7-p8 project (e.g. cloned from the **magento247-p8** repo).  
  This directory contains Magento’s `composer.json`, `app/`, `bin/`, `pub/`, etc.
- `conf/php/` – PHP Dockerfile and `php.ini` used to build the PHP-FPM and cron images.
- `conf/nginx/nginx.conf` – Nginx virtual host configuration.

## Prerequisites

- Docker and Docker Compose installed on your machine.
- A Magento 2.4.7-p8 codebase (such as **magento247-p8**) available.

## Getting Started

1. Clone this repository:
   - `git clone https://github.com/leonmex/magento247-p8.git magento247`
2. Place your Magento 2.4.7-p8 project into `code/` (for example, clone **magento247-p8** directly into `code/`):
   - `cd magento247`
3. Build and Start the Docker environment:
   - `docker-compose build`
   - `docker-compose up -d`
4. Install Magento:
   - `docker-compose exec phpfpm bash -lc "cd /var/www/html && composer install-magento"`
5. Install sample data:
   - `docker-compose exec phpfpm bash -lc "cd /var/www/html && composer install-sample-data"`
6. Install Nbg modules:
   - `docker-compose exec phpfpm bash -lc "cd /var/www/html && composer install-nbg-modules"`

## Accessing the Store

Add an entry to your `/etc/hosts` (or OS equivalent):
  - `127.0.0.1 magento.local`

After installation completes, you can access:

- Storefront: `http://magento.local/`
- Admin: `http://magento.local/myadminpanel` (as configured during install).

