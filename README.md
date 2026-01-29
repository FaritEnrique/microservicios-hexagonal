# Arquitectura de Microservicios - Trabajo Práctico

Este proyecto implementa una arquitectura de microservicios utilizando Spring Boot, Docker y PostgreSQL, siguiendo una arquitectura hexagonal.

## Estructura del Proyecto

El proyecto consta de tres microservicios:

*   **user-service:** Gestión de usuarios. Puerto: `8081`
*   **product-service:** Gestión de productos. Puerto: `8082`
*   **order-service:** Gestión de pedidos. Puerto: `8083`

## Requisitos Previos

*   Java 17
*   Maven
*   Docker y Docker Compose

## Configuración y Ejecución

### 1. Ejecución con Docker Compose (Recomendado)

Para levantar todo el entorno (microservicios y bases de datos) de una sola vez:

```bash
docker compose up --build
```

Esto iniciará:
*   3 contenedores de PostgreSQL (uno para cada servicio).
*   Los 3 microservicios conectados a sus respectivas bases de datos.

### 2. Ejecución Local (Desarrollo)

Cada microservicio tiene dos perfiles configurados:

*   **local:** Usa base de datos H2 en memoria. Ideal para desarrollo rápido y pruebas.
    *   Para ejecutar: `mvn spring-boot:run -Dspring-boot.run.profiles=local`
*   **dev:** Usa base de datos PostgreSQL. Requiere tener las bases de datos levantadas o configurar las variables de entorno.

### 3. Variables de Entorno

Para el perfil `dev`, los servicios esperan las siguientes variables de entorno (ya configuradas en `docker-compose.yml`):

*   `DB_URL`
*   `DB_USERNAME`
*   `DB_PASSWORD`

Puedes crear un archivo `.env` en la raíz de cada servicio basado en `.env.example` (si existe) para ejecutar localmente con perfil `dev`.

## Endpoints Principales

*   **User Service:** `/api/users`
*   **Product Service:** `/api/products`
*   **Order Service:** `/api/orders`

## Pruebas

Para ejecutar las pruebas unitarias y de integración:

```bash
mvn test
```

## CI/CD

Cada microservicio incluye un `Jenkinsfile` configurado para:
1.  Compilar el proyecto.
2.  Ejecutar pruebas.
3.  Construir y publicar la imagen Docker (requiere configuración de credenciales en Jenkins).
