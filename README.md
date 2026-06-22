# Backend Ventas – Spring Boot API REST

Servicio backend desarrollado con **Spring Boot** que expone una API REST para la gestión de ventas.  
Forma parte del proyecto DevOps P3 y se conecta a una base de datos MySQL desplegada en AWS u otro entorno.

## Requisitos

- Java 17
- Maven
- Base de datos MySQL accesible desde el servicio

## Configuración de la aplicación

El archivo `src/main/resources/application.properties` está parametrizado con variables de entorno:

```properties
spring.datasource.url=jdbc:mysql://${DB_ENDPOINT}:${DB_PORT}/${DB_NAME}?useSSL=false&serverTimezone=UTC&createDatabaseIfNotExist=true
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=update
```

Variables de entorno esperadas:

- `DB_ENDPOINT`
- `DB_PORT`
- `DB_NAME`
- `DB_USERNAME`
- `DB_PASSWORD`

Opcionalmente se puede definir `server.port` (por defecto 8080).

## Ejecución en entorno local

```bash
mvn spring-boot:run
```

La API queda disponible en `http://localhost:8080`.

## Swagger / documentación

La documentación de la API está disponible en:

- `http://localhost:8080/swagger-ui.html`

## Imagen Docker

El proyecto incluye un `Dockerfile` basado en Maven + JDK para construir y ejecutar la API:

```bash
# Construir la imagen
docker build -t devops-p3-backend-ventas .

# Ejecutar el contenedor
docker run -d \
  -p 8080:8080 \
  -e DB_ENDPOINT=<host-db> \
  -e DB_PORT=3306 \
  -e DB_NAME=<nombre-db> \
  -e DB_USERNAME=<usuario> \
  -e DB_PASSWORD=<password> \
  devops-p3-backend-ventas
```

El servicio expone el puerto **8080** en el contenedor.

## Integración en CI/CD

El repositorio incluye un workflow `deploy.yml` en `.github/workflows` que:

- Se ejecuta al hacer push sobre la rama `deploy`.
- Construye la imagen Docker del backend.
- Publica la imagen en un registry (por ejemplo Amazon ECR).
- Deja preparado el paso de despliegue automático en AWS/EKS/EC2.