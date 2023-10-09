Docker Compose allows you to define multi-container applications and specify how they communicate with each other using Docker networks. Below is an example of a Docker Compose file that defines two services (containers) and sets up a custom network for them to communicate:

```yaml
version: '3'

services:
  webapp:
    image: nginx:latest
    container_name: webapp-container
    ports:
      - "80:80"
    networks:
      - my-network

  database:
    image: mysql:latest
    container_name: db-container
    environment:
      MYSQL_ROOT_PASSWORD: root_password
    networks:
      - my-network

networks:
  my-network:
```

In this Docker Compose file:

1. We define two services: `webapp` and `database`.

2. The `webapp` service uses the official Nginx image and maps port 80 from the container to port 80 on the host machine.

3. The `database` service uses the official MySQL image and sets the root password as an environment variable.

4. We define a custom network named `my-network` under the `networks` section.

5. Both services (`webapp` and `database`) are connected to the `my-network` network using the `networks` directive within their respective service definitions.

By placing both containers on the same custom network (`my-network`), they can communicate with each other using their service names as hostnames. For example, the `webapp` service can connect to the MySQL database using the hostname `database`.

To start these containers using Docker Compose, navigate to the directory containing the `docker-compose.yml` file and run:

```bash
docker-compose up -d
```

This will create and start the `webapp-container` and `db-container` containers on the `my-network` network, allowing them to communicate.

Inside the `webapp` container, you can use the hostname `database` to connect to the MySQL database. For example, in a configuration file or code that requires the MySQL host:

```plaintext
# MySQL connection settings
host: database
port: 3306
user: root
password: root_password
```

This setup simplifies container communication and isolates the services within the same network while keeping them separate from other containers running on the host machine.
