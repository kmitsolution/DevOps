Certainly, let's create a real-time example where we have two containers, one for a Python application and another for a Node.js application, both using the same MySQL database. We'll use Docker Compose to set up this scenario.

Here's the directory structure and files for the example:

```plaintext
myapp/
│
├── docker-compose.yml
├── python-app/
│   ├── Dockerfile
│   └── app.py
│
├── node-app/
│   ├── Dockerfile
│   └── app.js
│
└── init.sql
```

1. `docker-compose.yml` - Defines the services and their configurations.
2. `python-app/Dockerfile` - Dockerfile for the Python application.
3. `python-app/app.py` - Python application code.
4. `node-app/Dockerfile` - Dockerfile for the Node.js application.
5. `node-app/app.js` - Node.js application code.
6. `init.sql` - SQL script to initialize the MySQL database.

Now, let's configure these files:

`docker-compose.yml`:

```yaml
version: '3'

services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: mydb
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"

  python-app:
    build:
      context: ./python-app
    container_name: python-app-container
    depends_on:
      - mysql
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: root_password
      MYSQL_DB: mydb

  node-app:
    build:
      context: ./node-app
    container_name: node-app-container
    depends_on:
      - mysql
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: root_password
      MYSQL_DB: mydb

volumes:
  mysql-data:
```

`python-app/Dockerfile`:

```Dockerfile
FROM python:3.8

WORKDIR /app

COPY . .

RUN pip install mysql-connector-python

CMD ["python", "app.py"]
```

`python-app/app.py` (Python application code):

```python
import mysql.connector

MYSQL_HOST = "mysql"
MYSQL_USER = "root"
MYSQL_PASSWORD = "root_password"
MYSQL_DB = "mydb"

try:
    connection = mysql.connector.connect(
        host=MYSQL_HOST,
        user=MYSQL_USER,
        password=MYSQL_PASSWORD,
        database=MYSQL_DB
    )

    cursor = connection.cursor()

    # Create a table if it doesn't exist
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS employees (
            id INT AUTO_INCREMENT PRIMARY KEY,
            first_name VARCHAR(255),
            last_name VARCHAR(255),
            email VARCHAR(255)
        )
    """)

    connection.commit()

    print("Table 'employees' created or already exists.")

except Exception as e:
    print("Error:", str(e))

finally:
    if connection.is_connected():
        cursor.close()
        connection.close()
```

`node-app/Dockerfile`:

```Dockerfile
FROM node:14

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "app.js"]
```

`node-app/app.js` (Node.js application code):

```javascript
const mysql = require('mysql');

const MYSQL_HOST = "mysql";
const MYSQL_USER = "root";
const MYSQL_PASSWORD = "root_password";
const MYSQL_DB = "mydb";

const connection = mysql.createConnection({
    host: MYSQL_HOST,
    user: MYSQL_USER,
    password: MYSQL_PASSWORD,
    database: MYSQL_DB
});

connection.connect((err) => {
    if (err) {
        console.error('Error connecting to MySQL:', err);
        return;
    }
    console.log('Connected to MySQL database');
});

// Define your Node.js application logic here
// For example, you can perform database queries, etc.

connection.end();
```

In this example:

- We have a MySQL service defined in `docker-compose.yml`, which sets up the MySQL container with a root password and a database named `mydb`. We also map port 3306 to allow connections to MySQL from the host.

- The Python and Node.js applications (defined in their respective Dockerfiles and code files) connect to the MySQL database using the provided environment variables (e.g., `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`) in the `docker-compose.yml` file.

- Both applications create or use the `employees` table in the `mydb` database. You can replace the application logic with your specific use case.

To run this example, navigate to the directory containing `docker-compose.yml` and run:

```bash
docker-compose up -d
```

This will start the MySQL container and both the Python and Node.js application containers. They will share the same MySQL database (`mydb`) for their data storage needs.
