# Arquitectura de Microservicios - Trabajo Práctico

Este proyecto implementa una arquitectura de microservicios utilizando Spring Boot, Docker y PostgreSQL, siguiendo una arquitectura hexagonal.

## Estructura del Proyecto

El proyecto consta de tres microservicios:

*   **user-service:** Gestión de usuarios. Puerto: `8081`
*   **product-service:** Gestión de productos. Puerto: `8082`
*   **order-service:** Gestión de pedidos. Puerto: `8083`

## Enlaces a Docker Hub (Entregable)

Aquí se encuentran las imágenes oficiales construidas y publicadas:

*   **User Service:** [https://hub.docker.com/r/faritenrique/user-service](https://hub.docker.com/r/faritenrique/user-service)
*   **Product Service:** [https://hub.docker.com/r/faritenrique/product-service](https://hub.docker.com/r/faritenrique/product-service)
*   **Order Service:** [https://hub.docker.com/r/faritenrique/order-service](https://hub.docker.com/r/faritenrique/order-service)

## Requisitos Previos

*   Java 17
*   Maven
*   Docker y Docker Compose

## Configuración y Ejecución

### 1. Configuración de Variables (.env)

Antes de iniciar, crea un archivo `.env` en la raíz del proyecto copiando el ejemplo:

```bash
cp .env.example .env
```

Esto configurará las credenciales de base de datos para Docker Compose.

### 2. Ejecución con Docker Compose (Recomendado)

Para levantar todo el entorno (microservicios y bases de datos) de una sola vez:

```bash
docker compose up --build
```

Esto iniciará:
*   3 contenedores de PostgreSQL.
*   Los 3 microservicios conectados a sus respectivas bases de datos (perfil `dev`).

### 3. Build & Push Manual a Docker Hub

Si deseas subir las imágenes manualmente a Docker Hub (aunque el Pipeline de Jenkins ya lo hace):

```bash
# Login en Docker Hub
docker login

# User Service
docker build -t faritenrique/user-service:latest ./user-service
docker push faritenrique/user-service:latest

# Product Service
docker build -t faritenrique/product-service:latest ./product-service
docker push faritenrique/product-service:latest

# Order Service
docker build -t faritenrique/order-service:latest ./order-service
docker push faritenrique/order-service:latest
```

## Endpoints Principales

*   **User Service:** `/api/users`
*   **Product Service:** `/api/products`
*   **Order Service:** `/api/orders`

## Monitoreo (Actuator)

Cada microservicio expone un endpoint de salud para verificar su estado:
*   `http://localhost:8081/actuator/health`
*   `http://localhost:8082/actuator/health`
*   `http://localhost:8083/actuator/health`

## Pruebas

Para ejecutar las pruebas unitarias y de integración:

```bash
mvn test
```

## CI/CD

Cada microservicio incluye un `Jenkinsfile` configurado para ejecutarse en un entorno Jenkins con Docker.
El pipeline realiza los siguientes pasos:
1.  **Checkout:** Obtiene el código fuente.
2.  **Test & Build:** Ejecuta pruebas y compila el JAR.
3.  **Docker Build:** Construye la imagen con tag de versión y latest.
4.  **Docker Push:** Sube las imágenes a Docker Hub (requiere credencial `dockerhub-creds`).
