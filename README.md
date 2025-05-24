# Odoo ERP

This repository contains a Docker-based development environment for Odoo ERP.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Directory Structure](#directory-structure)
- [Installation](#installation)
  - [Clone the Repository](#clone-the-repository)
  - [Initialize Git Submodules](#initialize-git-submodules)
  - [Configure Environment](#configure-environment)
  - [Build and Start](#build-and-start)
- [Configuration](#configuration)
  - [Environment Variables](#environment-variables)
  - [Resource Allocation](#resource-allocation)
- [Usage](#usage)
  - [Starting the Environment](#starting-the-environment)
  - [Stopping the Environment](#stopping-the-environment)
  - [Viewing Logs](#viewing-logs)
  - [Accessing the Database](#accessing-the-database)
- [Development](#development)
  - [Adding Custom Modules](#adding-custom-modules)
  - [Installing Modules](#installing-modules)
  - [Database Management](#database-management)
- [Troubleshooting](#troubleshooting)
  - [Common Issues](#common-issues)

## Overview

This workspace provides a complete development environment for Odoo ERP applications using Docker and Docker Compose. The setup includes:

- Odoo application server
- PostgreSQL 16 database
- Resource-optimized configuration
- Development tools and addons support

## Features

- 🐳 Containerized development environment
- 🚀 Resource-optimized for lower-end systems
- 🔒 Environment variable configuration for security
- 📦 Easy custom addon management
- ⚙️ Pre-configured PostgreSQL with performance tuning

## Prerequisites

- Docker 20.10.x or newer
- Docker Compose v2.x
- Git

## Directory Structure

```
odoo-docker/
├── .docker/                  # Docker configuration files
│   ├── Dockerfile           # Odoo container definition
│   ├── entrypoint.sh        # Container startup script
│   ├── odoo.conf            # Odoo configuration
│   └── wait-for-psql.py     # Database connection utility
├── addons/                   # Custom Odoo modules/addons
│   ├── oca/                 # OCA community addons (submodules)
│   └── asaidgroup/          # ASAid custom addons (submodules)
├── .env                      # Environment variables (excluded from git)
├── .gitignore                # Git ignore rules
├── .gitmodules               # Git submodule definitions
├── docker-compose.yml        # Docker Compose configuration
└── README.md                 # This file
```

## Installation

### Clone the Repository

```bash
git clone <repository-url>
cd odoo-docker
```

### Initialize Git Submodules

```bash
git submodule init
git submodule update --recursive --remote
```

> **Important**: This step is mandatory as it fetches all the necessary Odoo addon modules defined in `.gitmodules`. The docker container will not function correctly without these submodules properly initialized.

### Configure Environment

```bash
cp .env.example .env
# Edit .env with your preferred settings
```

### Build and Start

```bash
docker-compose up -d
```

Access Odoo at http://localhost:8069

## Configuration

### Environment Variables

The .env file contains all configurable parameters:

| Variable                 | Default       | Description                  |
| ------------------------ | ------------- | ---------------------------- |
| ODOO_VERSION             | 17            | Odoo version                 |
| ODOO_DB_HOST             | db            | Database hostname            |
| ODOO_DB_USER             | odoo          | Database username            |
| ODOO_DB_PASSWORD         | odoo_password | Database password            |
| POSTGRES_DB              | postgres      | PostgreSQL database name     |
| POSTGRES_USER            | odoo          | PostgreSQL username          |
| POSTGRES_PASSWORD        | odoo_password | PostgreSQL password          |
| POSTGRES_SHARED_BUFFERS  | 128MB         | PostgreSQL memory allocation |
| POSTGRES_MAX_CONNECTIONS | 20            | Max concurrent connections   |

### Resource Allocation

Both services are configured with resource limits:

- **Odoo**: 2 CPU threads, 1GB RAM
- **PostgreSQL**: 2 CPU threads, 512MB RAM

To modify these limits, edit the `deploy.resources.limits` section in docker-compose.yml.

## Usage

### Starting the Environment

```bash
docker-compose up -d
```

### Stopping the Environment

```bash
docker-compose down
```

### Viewing Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f odoo
```

### Accessing the Database

```bash
docker-compose exec db psql -U odoo postgres
```

## Development

### Adding Custom Modules

Place your custom modules in the addons directory. Odoo will automatically detect them.

If adding a new git submodule:

```bash
# Add a new submodule
git submodule add -b [branch-name] [repository-url] addons/[path]

# Example:
git submodule add -b 17.0 git@github.com:OCA/web.git addons/oca/web
```

### Installing Modules

1. Access Odoo at http://localhost:8069
2. Create a new database or access an existing one
3. Go to Apps → Update Apps List
4. Install your custom modules

### Database Management

Access the database manager at http://localhost:8069/web/database/manager

## Troubleshooting

### Common Issues

1. **Permission Denied Errors**

  ```bash
  sudo chown -R $USER:$USER .docker addons
  ```

2. **Port Conflicts**

  ```bash
  # Edit the port mappings in docker-compose.yml
  # Example: "8080:8069" instead of "8069:8069"
  ```

3. **Database Connection Issues**

  ```bash
  # Check PostgreSQL logs
  docker-compose logs db
  ```

4. **Resource Limitations**

  If Odoo performance is poor, try increasing the resource limits in docker-compose.yml and adjusting the corresponding settings in odoo.conf.

5. **Missing Addons/Modules**

  ```bash
  # Make sure submodules are properly initialized
  git submodule status
  
  # If any are missing, run:
  git submodule update --init --recursive
  ```

---

Maintained by ASAid Group Investment <developer@asaidgroup.com>