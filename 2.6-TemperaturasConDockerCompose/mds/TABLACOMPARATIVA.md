# Tarea 5.2: Tabla Comparativa

| Aspecto | Temperaturas manual (Docker CLI)                                                        | Temperaturas Compose                                                                             |
| :--- |:----------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------|
| **Comandos para desplegar** | Múltiples comandos (`docker network create`, `docker run`).                             | Un único comando: `docker compose up -d`.                                                        |
| **Gestión de red** | Creación manual de la red `--network` en cada contenedor.                               | Docker Compose crea una red para el proyecto y asigna nombres DNS a los servicios.               |
| **Variables de entorno** | Manualmente usando múltiples flags `-e`.                                                | Se definen en el `docker-compose.yml` o se cargan desde un `.env`.                               |
| **Escalado** | Complejo.                                                                               | Se utiliza el flag `--scale servicio=N` o la directiva `replicas` en el archivo YAML.            |
| **Orden de inicio** | El usuario ejecuta los comandos en orden.                                   | Se define con `depends_on`.                                                                      |
| **Logs** | Se deben consultar contenedor por contenedor (`docker logs <nombre>`).                  | Permite verlos todos unificados en tiempo real (`docker compose logs -f`).                       |