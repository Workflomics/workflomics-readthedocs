Installation Guide
==================

This guide outlines the steps to install and deploy the Workflomics web platform, a comprehensive solution implemented in TypeScript for bioinformatics workflow benchmarking.

Development Setup
-----------------

**Prerequisites**:

- Ensure you have a Postgres database and Postgrest API set up and running. For a complete setup using Docker, refer to the Deployment section below.

**Environment Configuration**:

1. Create a `.env` file in the project directory and configure the proxy endpoints:

   .. code-block:: text

      API_PROXY_TARGET=http://localhost:3000
      APE_PROXY_TARGET=http://localhost:4444

**Installing Dependencies**:

2. Install the required modules for the front-end:

   .. code-block:: bash

      npm install

**Running the Front-End**:

3. To start the front-end in development mode, run:

   .. code-block:: bash

      npm start

Deployment
----------

**Back-End Services**:

1. **Setup**:

   - Copy `docker-compose.yml` to the server.
   - Create a `.env` file in the same directory and configure the necessary variables:

     .. code-block:: text

        POSTGRES_PASSWORD=<password>
        WF_DATA_DIR=<data directory>

   - Note: Ports are currently hard-coded in `docker-compose.yml`.

2. **Starting Services**:

   - To start the database, API, and RestAPE, execute:

     .. code-block:: bash

        docker compose --env-file .env up -d

   - To remove the containers, run:

     .. code-block:: bash

        docker compose down

**Nginx Configuration**:

- The front-end requires a reverse proxy, for which Nginx can be used. A sample Nginx configuration is available in the `nginx` folder. Ensure it points to the correct back-end services and is symlinked from `/etc/nginx/sites-available` to `/etc/nginx/sites-enabled`.

**Building and Deploying the Front-End**:

1. **Build Application**:

   - Ensure you're on the correct branch and have incorporated all desired changes. Build an optimized version of the application with:

     .. code-block:: bash

        npm run build

   - This command generates the application in the `build` directory, ready for static serving.

2. **Serving Application**:

   - For serving with Nginx, copy the contents of the `build/` directory to the Nginx serving directory, e.g., `/var/www/workflomics.org/`.

This guide provides a comprehensive approach to both developing and deploying the Workflomics platform, ensuring a smooth setup for your bioinformatics workflow benchmarking needs.
