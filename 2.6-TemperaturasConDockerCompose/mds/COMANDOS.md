# Comandos usados en el ejercicio

## Despliegue y Monitorización

- **`docker compose up -d`** Lee el archivo YAML y levanta los servicios en segundo plano.

- **`docker compose ps`** Lista los contenedores del proyecto, mostrando puertos y estado.

- **`docker compose logs -f`** Muestra los logs de todos los servicios y espera nuevos mensajes.

- **`docker compose logs -f frontend`** Filtra y muestra únicamente los logs del servicio frontend.

- **`docker compose logs -f backend`** Filtra y muestra únicamente los logs del servicio backend.

---

## Pruebas de Resiliencia

- **`docker compose stop backend`** Detiene el contenedor del backend para simular un fallo.

- **`docker compose start backend`** Arranca de nuevo el contenedor detenido para recuperar el servicio.

---

## Escalado y Limpieza

- **`docker compose up -d --scale backend=3`** Actualiza el despliegue lanzando 3 réplicas del servicio backend.

- **`docker compose down`** Detiene y elimina todos los contenedores y la red creada.