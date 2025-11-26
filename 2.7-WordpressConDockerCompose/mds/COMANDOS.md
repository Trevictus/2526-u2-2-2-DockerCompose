# Comandos usados en el ejercicio

## Preparación del Entorno
- `mkdir ~/wordpress_bind`  
  Crea el directorio principal para la práctica con **bind mounts**.

- `mkdir wordpress_files mysql_files`  
  Crea las carpetas locales donde se guardarán los datos persistentes del host.

---

## Despliegue y Gestión
- `docker compose up -d`  
  Levanta los servicios de **WordPress** y **MariaDB** en segundo plano basándose en el archivo YAML.

- `docker compose ps`  
  Muestra el estado de los contenedores y si están **healthy** (sanos) o **unhealthy**.

- `docker compose stop`  
  Detiene los servicios sin eliminar los contenedores ni los datos.

- `docker compose start`  
  Vuelve a arrancar los servicios previamente detenidos.

---

## Diagnóstico y Healthchecks
- `docker compose logs db` Muestra los logs específicos del contenedor.

- `docker inspect --format "{{json .State.Health}}" wordpress_bind-db-1` Muestra el reporte detallado del estado de salud.

---

## Limpieza y Recreación
- `docker compose down` Detiene y elimina los contenedores y las redes, pero mantiene los volúmenes de datos.

- `docker compose down -v` Detiene y elimina todo, incluyendo los volúmenes.

---

## Backups y Restauración
- `docker run --rm -v wordpress_compose_wp_data:/volume -v $(pwd):/backup alpine tar cvf /backup/wp_backup.tar /volume` Crea una copia de seguridad de un volumen Docker comprimiéndolo en un archivo **tar** mediante un contenedor temporal.

- `tar -czvf backup_completo.tar.gz ./wordpress_files ./mysql_files` Crea una copia de seguridad comprimida de las carpetas locales usadas en los **bind mounts**.