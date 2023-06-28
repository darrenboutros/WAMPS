# Docker Compose Configuration
This repository contains a Docker Compose configuration file (**`docker-compose.yml`**) to set up a multi-container environment for PHP development. It includes the following services:

- **web**: A PHP 8 and Apache container for hosting your PHP application.
- **db**: A PostgreSQL container for your database needs.
- **phpmyadmin**: A PHPMyAdmin container for managing your PostgreSQL database.
- **pgadmin**: A PgAdmin container for managing your PostgreSQL server.
## Prerequisites
Before using this Docker Compose configuration, please ensure that you have the following prerequisites installed on your system:

- Docker Engine
- Docker Compose
## Usage
1. Clone this repository to your local machine.
1. Open a terminal or command prompt and navigate to the cloned repository's directory.
1. Customize the environment variables in the docker-compose.yml file according to your needs. Make sure to replace the placeholders **`${POSTGRES_DB}`**, **`${POSTGRES_USER}`**, **`${POSTGRES_PASSWORD}`**, **`${PGADMIN_DEFAULT_EMAIL}`**, and **`${PGADMIN_DEFAULT_PASSWORD}`** with your desired values.
1. Run the following command to start the containers:
	```
	docker-compose up -d
	```
1. Wait for the containers to be pulled and started. Once done, you can access the following services:
	- PHP application: http://localhost
	- PMyAdmin: http://localhost:8080
	- PgAdmin: http://localhost:5050
1. To stop and remove the containers, use the following command: 
	```
	docker-compose down
	```
## Configuration
### Web Service
- Container Name: web
- Image: php:8-apache
- Ports: 80:80 (Maps host port 80 to container port 80)
- Volumes: ./app:/var/www/html (Mounts the local **`app`** directory to the container's **`/var/www/html`** directory)
- Restart Policy: always (Restarts the container automatically if it stops unexpectedly)
- Networks: mynetwork (Connects the container to the custom network **`mynetwork`**)
### DB Service
- Container Name: postgres
- Image: postgres
- Environment Variables:
	- POSTGRES_DB: **`${POSTGRES_DB}`**
	- POSTGRES_USER: **`${POSTGRES_USER}`**
	- POSTGRES_PASSWORD: **`${POSTGRES_PASSWORD}`**
- Ports: 5432:5432 (Maps host port 5432 to container port 5432)
- Volumes: ./db-data:/var/lib/postgresql/data (Mounts the local **`db-data`** directory to the container's **`/var/lib/postgresql/data`** directory)
- Restart Policy: always
- Networks: mynetwork
### PHPMyAdmin Service
- Container Name: phpmyadmin
- Image: phpmyadmin/phpmyadmin
- Links: db (Links to the **`db`** service)
- Ports: 8080:80 (Maps host port 8080 to container port 80)
- Environment Variables:
	- PMA_HOST: db (Sets the host name of the PostgreSQL server)
	- PMA_PORT: 5432 (Sets the port number of the PostgreSQL server)
	- PMA_ARBITRARY: 1 (Enables connecting to arbitrary servers)
- Restart Policy: always
- Networks: mynetwork
### PgAdmin Service
- Container Name: pgadmin
- Image: dpage/pgadmin4
- Links: db (Links to the **`db`** service)
- Ports: 5050:80 (Maps host port 5050 to container port 80)
- Environment Variables:
	- PGADMIN_DEFAULT_EMAIL: **`${PGADMIN_DEFAULT_EMAIL}`** (Sets the default email for PgAdmin)
	- PGADMIN_DEFAULT_PASSWORD: **`${PGADMIN_DEFAULT_PASSWORD}`** (Sets the default password for PgAdmin)
- Restart Policy: always
- Networks: mynetwork
## Customization
Feel free to modify the configuration according to your specific requirements. You can add additional services, adjust ports, volumes, and environment variables as needed.

If you have any questions or issues, please don't hesitate to reach out.

Happy coding!