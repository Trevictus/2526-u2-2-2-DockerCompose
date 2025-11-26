# Tarea 5.1: Preguntas de Análisis

## 1. Comparación con práctica anterior

**¿Qué comandos de la Práctica 2.2 sustituye Docker Compose?**  
Docker Compose simplifica el trabajo, sustituyendolo todo por un solo comando.

* **Comandos sustituidos:**
    * Creación de red: `docker network create ...`
    * Arranque del backend: `docker run -d --name backend --network ...`
    * Arranque del frontend: `docker run -d -p 8081:3000 --name frontend --network ... -e ...`
    * Limpieza manual: `docker stop`, `docker rm` y `docker network rm`.

* **Sustitutos en Compose:**
    * Despliegue: `docker compose up -d`
    * Limpieza: `docker compose down`

**¿Es más fácil gestionar esta aplicación con Compose?**  
**Sí.** En lugar de recordar parámetros complejos en la línea de comandos, toda la configuración está en el archivo `docker-compose.yml`. Esto reduce errores.

**¿Qué ventajas adicionales obtienes?**
* **Resolución de DNS:** Los contenedores se reconocen por el nombre del servicio.
* **Gestión:** Permite iniciar, parar y reiniciar todos los servicios conjuntamente.
* **Escalado:** Se pueden crear réplicas con un simple flag (`--scale`).
* **Logs:** Permite ver la salida de todos los microservicios en una sola terminal.

---

## 2. Dependencias entre servicios

**¿Qué diferencia hay entre `depends_on` y `links`?**
* **`links`:** Es estática y no se recomienda su uso.
* **`depends_on`:** Es la actual para definir el inicio y apagado entre servicios.

**¿`depends_on` garantiza que el servicio esté listo para recibir peticiones?**  
**No.** `depends_on` solo garantiza que el contenedor se ha iniciado. No verifica si la aplicación interna ha terminado de cargar.

**¿Cómo se podría mejorar esto con healthchecks?**  
Se debe combinar `depends_on` con una condición de salud:

```yaml
depends_on:
  backend:
    condition: service_healthy
```

---

## 3. Arquitectura de microservicios

**¿Qué ventajas tiene separar frontend y backend?**
- **Desacoplamiento:** Permite ciclos de desarrollo independientes. Se puede actualizar el frontend sin detener el backend.
- **Escalabilidad selectiva:** Si el backend consume mucha CPU procesando datos, se puede escalar solo ese servicio sin duplicar el frontend.

**¿Cómo facilita Docker Compose el desarrollo de microservicios?**  
Permite simular la arquitectura en la máquina local con un solo comando. Esto asegura que el entorno de desarrollo sea idéntico al de producción.

**¿Qué pasa si el backend falla? ¿Y si el frontend falla?**
- **Backend:** El frontend sigue cargando y mostrando la interfaz, pero las funcionalidades que requieren datos fallarán.
- **Frontend:** La aplicación se cae totalmente. Aunque el backend funcione, no hay interfaz para acceder a él.

---

## 4. Escalado

**¿Por qué es más fácil escalar el backend que el frontend?**  
El backend es un servicio interno y sin estado que no expone puertos al host. Por tanto, puedes lanzar 10 réplicas sin conflictos. El frontend, en cambio, está mapeado a un puerto físico del host. Si intentas lanzar una segunda instancia, fallará porque el puerto 8081 ya está ocupado.

**¿Cómo se distribuirían las peticiones con múltiples backends?**  
Docker utiliza un DNS interno. Cuando el frontend resuelve el nombre `backend`, Docker devuelve la IP de los diferentes contenedores, distribuyendo la carga.

**¿Necesitarías un balanceador de carga?**
- **Para el Backend:** No es necesario uno externo la red de Docker es suficiente.
- **Para el Frontend:** Sí. Si escalas el frontend en varios puertos, necesitas un balanceador externo para dar una URL al usuario y repartir el tráfico.