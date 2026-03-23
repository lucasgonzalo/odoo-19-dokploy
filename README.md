# Odoo 19 Dockerized for Dokploy

This repository contains a Dockerized Odoo 19 setup optimized for deployment on **Dokploy**, but also fully functional for local development.

## Features
- **Odoo 19.0** (Official Image)
- **PostgreSQL 17** (Alpine)
- **Dokploy Ready**: Pre-configured with Traefik labels and external network support.
- **Optimized Healthchecks**: Uses Python-based healthchecks for reliable service monitoring.
- **Proxy Mode**: Enabled by default for correct header handling behind Traefik.

## Local Deployment

1.  **Clone the repository**:
    ```bash
    git clone git@github.com:lucasgonzalo/odoo-19-dokploy.git
    cd odoo-19-dokploy
    ```

2.  **Configure Environment Variables**:
    Create a `.env` file in the root directory:
    ```env
    POSTGRES_USER=odoo
    POSTGRES_PASSWORD=odoo
    POSTGRES_DB=postgres
    ODOO_MASTER_PASSWORD=your_master_password
    DOMAIN=localhost
    ```

3.  **Create the Dokploy Network** (if it doesn't exist):
    ```bash
    docker network create dokploy-network
    ```

4.  **Run with Docker Compose**:
    ```bash
    docker compose up -d
    ```

5.  **Access Odoo**:
    Open `http://localhost:8069` in your browser.

## Dokploy Deployment

1.  **Create a New Project** in Dokploy.
2.  **Add a Docker Compose Application**.
3.  **Connect your Repository**: Use `git@github.com:lucasgonzalo/odoo-19-dokploy.git`.
4.  **Set Environment Variables** in the Dokploy UI:
    - `POSTGRES_USER`
    - `POSTGRES_PASSWORD`
    - `POSTGRES_DB`
    - `ODOO_MASTER_PASSWORD`
    - `DOMAIN` (e.g., `odoo.yourdomain.com`)
5.  **Deploy**: Dokploy will automatically handle the Traefik routing and SSL certificates. Ensure you have configured your domain in the **"Domains"** tab of the Dokploy application.

## Configuration

The Odoo configuration is located in `config/odoo.conf`. It is pre-configured for:
- `proxy_mode = True`
- `workers = 4`
- Correct memory limits for production.

> [!IMPORTANT]
> The `admin_passwd` (Master Password) is set via the `ODOO_MASTER_PASSWORD` environment variable in `docker-compose.yml`, not in the `odoo.conf` file.

## Volumes
- `odoo-web-data`: Stores Odoo's filestore and sessions.
- `postgres-data`: Stores the PostgreSQL database data.
