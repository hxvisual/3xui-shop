
# Gemini Code Assistant Context

This document provides context for the Gemini Code Assistant to understand the 3xui-shop project.

## Project Overview

3xui-shop is a Python-based Telegram bot designed for selling VPN subscriptions. It integrates with the 3X-UI panel for managing users and subscriptions. The bot is built using the `aiogram` framework and relies on `SQLAlchemy` for database interactions, `Alembic` for migrations, and `Redis` for caching and session storage. The entire application is containerized using Docker and uses Traefik as a reverse proxy for handling HTTPS requests.

### Core Technologies

*   **Programming Language:** Python 3.12
*   **Telegram Bot Framework:** `aiogram`
*   **Database:** `SQLAlchemy` with `aiosqlite` (likely, but configurable)
*   **Database Migrations:** `Alembic`
*   **Caching/Session Storage:** `Redis`
*   **Deployment:** `Docker` and `docker-compose`
*   **Reverse Proxy:** `Traefik`
*   **Dependency Management:** `Poetry`

### Architecture

The project follows a modular architecture:

*   **`app/`**: The main application directory.
    *   **`bot/`**: Contains all the bot-related logic, including:
        *   `routers/`: Defines the bot's command and message handlers.
        *   `services/`: Implements the business logic for features like subscriptions, referrals, and notifications.
        *   `payment_gateways/`: Integrates with various payment providers.
        *   `models/`: Defines data structures used within the bot.
    *   **`db/`**: Manages the database, including:
        *   `models/`: Contains the SQLAlchemy database models.
        *   `migration/`: Holds the Alembic migration scripts.
    *   **`locales/`**: Contains localization files for multi-language support.
    *   **`__main__.py`**: The main entry point of the application, responsible for initializing the bot, database, and other services.
    *   **`config.py`**: Defines the configuration structure and loads settings from environment variables.
*   **`docker-compose.yml`**: Defines the services required for deployment, including the bot, Redis, and Traefik.
*   **`pyproject.toml`**: Manages the project's Python dependencies using Poetry.
*   **`plans.json`**: A configuration file for defining the available subscription plans.

## Building and Running

The project is designed to be run with Docker.

### Prerequisites

*   Docker
*   Docker Compose

### Running the Application

1.  **Set up the environment:**
    *   Copy `.env.example` to `.env` and fill in the required environment variables.
    *   Copy `plans.example.json` to `plans.json` and configure your subscription plans.

2.  **Build and run the Docker containers:**
    ```bash
    docker compose build
    docker compose up -d
    ```

The `docker-compose.yml` file defines the following services:

*   **`bot`**: The main application container.
*   **`redis`**: The Redis server for caching and session storage.
*   **`traefik`**: A reverse proxy for handling HTTPS and routing traffic to the bot.

The entry point for the bot container executes the following commands:

1.  `poetry run pybabel compile -d /app/locales -D bot`: Compiles the translation files.
2.  `poetry run alembic -c /app/db/alembic.ini upgrade head`: Applies the latest database migrations.
3.  `poetry run python /app/__main__.py`: Starts the bot.

## Development Conventions

*   **Configuration:** All configuration is managed through environment variables, as defined in `app/config.py`.
*   **Database:** The project uses SQLAlchemy as an ORM and Alembic for database migrations. Database models are defined in `app/db/models/`.
*   **Bot Logic:** The bot's logic is organized into routers, services, and filters within the `app/bot/` directory.
*   **Dependencies:** Python dependencies are managed with Poetry and are listed in `pyproject.toml`.
*   **Localization:** The bot supports multiple languages using `pybabel`. Translation files are located in `app/locales/`.
*   **Coding Style:** The code follows standard Python conventions (PEP 8).
