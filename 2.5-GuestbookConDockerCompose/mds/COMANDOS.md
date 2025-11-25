# Comandos usados en el ejercicio

## Gestión de contenedores y servicios

- **`docker compose up -d`**  
  Crea red, volúmenes y contenedores en segundo plano.

- **`docker compose ps`**  
  Muestra qué contenedores de **este proyecto** están vivos.

- **`docker compose config`**  
  Muestra cómo queda el YAML final (con variables sustituidas).

- **`docker compose logs -f`**  
  Muestra la bitácora y se queda esperando nuevos mensajes.

---

## Control de procesos

- **`docker compose stop`**  
  Congela los procesos. No borra nada.

- **`docker compose start`**  
  Descongela los procesos detenidos.

- **`docker compose restart`**  
  Reinicia (apaga y prende rápido). Útil si la app se bloquea.

---

## Eliminación de recursos

- **`docker compose down`**  
  Elimina contenedores y redes. Mantiene datos.

- **`docker compose down -v`**  
  Elimina contenedores, redes y volúmenes (datos).