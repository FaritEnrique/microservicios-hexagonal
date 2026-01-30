# Arquitectura de Microservicios - Trabajo Práctico

Este proyecto implementa una arquitectura de microservicios utilizando Spring Boot, Docker y PostgreSQL, siguiendo una arquitectura hexagonal.

## Estructura del Proyecto

El proyecto consta de tres microservicios:

*   **user-service:** Gestión de usuarios. Puerto: `8081`
*   **product-service:** Gestión de productos. Puerto: `8082`
*   **order-service:** Gestión de pedidos. Puerto: `8083`

## Enlaces a Docker Hub (Entregable)

*   **User Service:** (Pegar enlace aquí)
*   **Product Service:** (Pegar enlace aquí)
*   **Order Service:** (Pegar enlace aquí)

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

Si deseas subir las imágenes manualmente a Docker Hub:

```bash
# Login en Docker Hub
docker login

# User Service
docker build -t TU_USUARIO/user-service:latest ./user-service
docker push TU_USUARIO/user-service:latest

# Product Service
docker build -t TU_USUARIO/product-service:latest ./product-service
docker push TU_USUARIO/product-service:latest

# Order Service
docker build -t TU_USUARIO/order-service:latest ./order-service
docker push TU_USUARIO/order-service:latest
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

Cada microservicio incluye un `Jenkinsfile` configurado para:
1.  Compilar el proyecto.
2.  Ejecutar pruebas.
3.  Construir y publicar la imagen Docker.

**Nota:** Debes configurar las credenciales `dockerhub-credentials` en tu servidor Jenkins y actualizar el `DOCKER_HUB_USERNAME` en cada `Jenkinsfile`.
