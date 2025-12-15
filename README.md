# Proyecto de Estudiante: Aplicación Web PHP + Nginx + MySQL con Docker Compose

Este proyecto es una implementación simple y limpia de una aplicación web que utiliza **PHP 8.3 (FPM)**, **Nginx** como servidor web y **MySQL 8.0** como base de datos, orquestados mediante **Docker Compose**. El objetivo es proporcionar un entorno de desarrollo local, replicable y fácil de mantener, siguiendo las mejores prácticas de Docker.

## Estructura del Proyecto

```
php-nginx-mysql-app/
├── .env                  # Variables de entorno sensibles
├── docker-compose.yml    # Definición de los servicios (orquestación)
├── Dockerfile            # Definición de la imagen del servicio PHP
├── nginx/
│   └── default.conf      # Configuración de Nginx para PHP-FPM
└── src/
    └── index.php         # Código PHP "vainilla" de la aplicación
```

## Problema Encontrado y Solución Lógica

**Problema:**
Durante la fase de validación, se detectó que el entorno de la caja de arena (sandbox) no tiene instalados los comandos `docker` ni `docker compose`. Esto impidió la construcción y el levantamiento de los contenedores para una prueba funcional directa.

**Solución Lógica (Priorizando la Sencillez):**
Dado que el requisito principal era **generar el código necesario** para que la aplicación funcione **de manera local** (como un proyecto de estudiante), se ha completado la generación de todos los archivos con las mejores prácticas. La validación se realiza mediante una revisión exhaustiva del código y la configuración. Se proporcionan instrucciones claras para que el usuario pueda ejecutar el proyecto en su máquina local, donde se asume que Docker está instalado.

## Explicación de las Buenas Prácticas Aplicadas

1.  **Separación de Servicios (Microservicios):**
    *   Se utiliza un contenedor para cada servicio principal (`app` para PHP-FPM, `nginx` para el servidor web, `db` para MySQL). Esto mejora la escalabilidad y el mantenimiento.
2.  **Uso de Imágenes Oficiales y Específicas:**
    *   Se utiliza `php:8.3-fpm-alpine` para el servicio PHP, que es una imagen oficial, ligera (basada en Alpine) y específica para la versión requerida.
    *   Se utiliza `nginx:stable-alpine` y `mysql:8.0`.
3.  **Gestión de Variables Sensibles (`.env`):**
    *   Las credenciales de la base de datos y el puerto de Nginx se gestionan a través del archivo `.env`. Este archivo se referencia en `docker-compose.yml` y **debe ser excluido del control de versiones (Git)** para evitar exponer secretos.
4.  **Volúmenes Persistentes:**
    *   Se define un volumen con nombre (`db_data`) para el servicio MySQL. Esto asegura que los datos de la base de datos persistan incluso si el contenedor `db` es detenido o eliminado, cumpliendo con el requisito de conservación de datos.
5.  **Comunicación entre Contenedores:**
    *   Los servicios se comunican a través de una red definida por Docker Compose (`app-network`). El servicio PHP se conecta a MySQL usando el nombre del servicio (`db`) como hostname, y Nginx se comunica con PHP-FPM usando el nombre del servicio (`app`) y el puerto 9000.

## Instrucciones de Uso Local

Para levantar esta aplicación en su entorno local, siga estos pasos:

1.  **Asegúrese de tener Docker y Docker Compose (o Docker CLI) instalados.**
2.  **Navegue al directorio raíz del proyecto** (`php-nginx-mysql-app`).
3.  **Ejecute el siguiente comando** para construir la imagen de PHP y levantar todos los servicios en segundo plano:

    ```bash
    docker compose up --build -d
    ```

4.  **Acceda a la aplicación** abriendo su navegador web en la siguiente dirección:

    ```
    http://localhost:8080
    ```

5.  **Para detener y eliminar los contenedores** (manteniendo el volumen de datos de MySQL), ejecute:

    ```bash
    docker compose down
    ```

6.  **Para detener y eliminar los contenedores y el volumen de datos persistentes** (lo que borrará la base de datos), ejecute:

    ```bash
    docker compose down -v
    ```

## Código PHP (`src/index.php`)

El código PHP es un ejemplo "vainilla" que utiliza la extensión **PDO** para conectarse a la base de datos MySQL. Demuestra:
*   Uso de variables de entorno para la configuración de la conexión.
*   Manejo básico de excepciones.
*   Creación de una tabla simple y una inserción de prueba.
*   Visualización del estado de la conexión y los datos de la base de datos.

Este código es intencionalmente simple para cumplir con el requisito de ser un proyecto de estudiante y evitar el uso de frameworks.
