# Docker Compose Setup for PHP and MySQL Blog

This repository contains a Docker Compose configuration to deploy a PHP application with an Apache web server and a MariaDB database.

## Services

- **web**: Runs a PHP application using the `php:apache` Docker image.
  - Container name: `php_blog`
  - Host port: `8082` mapped to container port `80`
  - Host volume: `/var/www/html` mapped to container volume `/var/www/html`

- **db**: Runs a MariaDB database using the `mariadb:latest` Docker image.
  - Container name: `mysql_blog`
  - Host port: `3306` mapped to container port `3306`
  - Host volume: `/var/lib/mysql` mapped to container volume `/var/lib/mysql`
  - Environment variables:
    - `MYSQL_DATABASE=database_blog`
    - `MYSQL_USER=customuser`
    - `MYSQL_PASSWORD=complexpassword`
    - `MYSQL_ROOT_PASSWORD=rootpassword`

## Prerequisites

- Docker installed on the host machine.
- Docker Compose installed on the host machine.

## Setup

1. **SSH into App Server 3:**
   ```sh
   ssh user@app-server-3.stratos-datacenter
   ```

2. **Ensure the necessary directories are created:**
   ```sh
   sudo mkdir -p /var/www/html
   sudo mkdir -p /var/lib/mysql
   ```

3. **Create the Docker Compose file:**
   ```sh
   sudo nano /opt/devops/docker-compose.yml
   ```

4. **Paste the following content into `docker-compose.yml`:**
   ```yaml
   version: '3.8'

   services:
     web:
       container_name: php_blog
       image: php:apache
       ports:
         - "8082:80"
       volumes:
         - /var/www/html:/var/www/html

     db:
       container_name: mysql_blog
       image: mariadb:latest
       ports:
         - "3306:3306"
       volumes:
         - /var/lib/mysql:/var/lib/mysql
       environment:
         MYSQL_DATABASE: database_blog
         MYSQL_USER: customuser
         MYSQL_PASSWORD: complexpassword
         MYSQL_ROOT_PASSWORD: rootpassword
   ```

5. **Save and exit the file:**
   - Press `Ctrl+X` to exit.
   - Press `Y` to confirm the changes.
   - Press `Enter` to save the file.

6. **Navigate to the directory containing the Docker Compose file:**
   ```sh
   cd /opt/devops
   ```

7. **Run the Docker Compose setup:**
   ```sh
   sudo docker-compose up -d
   ```

8. **Verify that the containers are running:**
   ```sh
   sudo docker ps
   ```

## Access the Application

After running `docker-compose up`, you can access the PHP application using the following command:

```sh
curl <server-ip-or-hostname>:8082/
```

Replace `<server-ip-or-hostname>` with the actual IP address or hostname of App Server 3.

## Notes

- Ensure you have appropriate permissions for the directories used by Docker volumes.
- Adjust the environment variables and other settings as per your requirements.

## Troubleshooting

- If the containers fail to start, check the Docker logs for more details:
  ```sh
  sudo docker-compose logs
  ```

- Ensure no other services are using the specified ports (`8082` for the web service and `3306` for the database service).