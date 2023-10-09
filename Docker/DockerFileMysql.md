Creating a Dockerfile for a MySQL database with tables involves several steps. Here's a basic example of a Dockerfile that sets up a MySQL container and creates a database table when the container is built and run:

```Dockerfile
# Use the official MySQL Docker image as the base image
FROM mysql:latest

# Set environment variables for MySQL root user password and database name
ENV MYSQL_ROOT_PASSWORD=root_password
ENV MYSQL_DATABASE=mydb

# Copy SQL script to initialize the database and table
COPY init.sql /docker-entrypoint-initdb.d/

# Expose the MySQL port
EXPOSE 3306
```

In this example:

1. We start with the official MySQL Docker image as the base image.

2. We set environment variables `MYSQL_ROOT_PASSWORD` and `MYSQL_DATABASE` to configure the MySQL server's root password and the name of the database you want to create.

3. We copy an SQL script named `init.sql` into the `/docker-entrypoint-initdb.d/` directory. This directory is special in the official MySQL image, and any SQL files placed here will be executed when the container is first started. You can customize `init.sql` with your specific SQL commands to create tables, insert data, etc. For example:

```sql
-- init.sql

-- Create a sample table
CREATE TABLE IF NOT EXISTS employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100)
);

-- Insert some sample data
INSERT INTO employees (first_name, last_name, email)
VALUES
    ('John', 'Doe', 'john.doe@example.com'),
    ('Jane', 'Smith', 'jane.smith@example.com');
```

4. We expose port 3306, which is the default MySQL port, to allow connections to the MySQL server from outside the container.

To build and run the Docker container using this Dockerfile:

1. Save the Dockerfile and the `init.sql` script in the same directory.

2. Open a terminal and navigate to the directory containing the Dockerfile and SQL script.

3. Build the Docker image:

```bash
docker build -t mysql-custom .
```

Replace `mysql-custom` with your desired image name.

4. Run the Docker container:

```bash
docker run -d -p 3306:3306 --name mysql-container mysql-custom
```

This command will create and start a Docker container named `mysql-container` based on the `mysql-custom` image, mapping port 3306 from the container to the host.

Now, you should have a MySQL container running with your custom database table inside it. You can connect to it using a MySQL client and the configured credentials.
