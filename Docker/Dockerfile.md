Certainly, here are some advanced Dockerfile examples for various use cases:

**1. Python Application with Virtual Environment:**

```Dockerfile
# Stage 1: Build the Python environment
FROM python:3.9 AS builder
WORKDIR /app
COPY requirements.txt .
RUN python -m venv venv
RUN venv/bin/pip install --no-cache-dir -r requirements.txt

# Stage 2: Create the production image
FROM python:3.9-slim
WORKDIR /app
COPY --from=builder /app/venv /app/venv
COPY . .
CMD ["/app/venv/bin/python", "app.py"]
```

This Dockerfile creates a two-stage build for a Python application. The first stage builds a virtual environment with dependencies, and the second stage copies the virtual environment and application code into the final image.

**2. Go Application with a Multi-Stage Build:**

```Dockerfile
# Stage 1: Build the Go binary
FROM golang:1.17 AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o myapp

# Stage 2: Create the production image
FROM alpine:3.14
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

This Dockerfile builds a Go application in the first stage and then uses a minimal Alpine Linux image for the production stage.

**3. Java Application with Maven:**

```Dockerfile
# Stage 1: Build the Java application
FROM maven:3.8-openjdk-11 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Create the production image
FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=builder /app/target/myapp.jar .
CMD ["java", "-jar", "myapp.jar"]
```

This Dockerfile builds a Java application using Maven and then uses a slim OpenJDK image for the production stage.

**4. Nginx Reverse Proxy for Node.js Application:**

```Dockerfile
# Stage 1: Build the Node.js application
FROM node:14 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Create the production image with Nginx
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

This Dockerfile builds a Node.js application in the first stage and configures an Nginx reverse proxy in the second stage.

**5. Multi-Service Docker Compose:**

```Dockerfile
# Dockerfile for a service named "web"
FROM nginx:alpine
COPY ./nginx.conf /etc/nginx/nginx.conf
# ...

# Dockerfile for a service named "api"
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
# ...
```

In a Docker Compose setup with multiple services, you can have separate Dockerfiles for each service, customizing each service's image as needed.

These examples demonstrate advanced Dockerfile techniques for various programming languages and use cases. You can further customize these examples based on your specific application requirements.
