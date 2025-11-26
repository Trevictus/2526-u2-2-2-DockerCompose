# Tarea 5.1: Preguntas de Análisis

## 1. Volúmenes vs. Bind Mounts

**¿Cuándo usaría volúmenes Docker?**  
En producción y para bases de datos como MariaDB. Son gestionados directamente por Docker, ofrecen mejor rendimiento y evitan problemas de permisos que suelen aparecer con los bind mounts. Están aislados del sistema de archivos del host reduciendo el riesgo de borrados accidentales.

**¿Cuándo usaría bind mounts?**  
En desarrollo local. Me permiten editar archivos de código directamente en mi ordenador y ver los cambios y reflejados en el contenedor, sin tener que reconstruir la imagen.

**¿Cuál es más fácil para hacer backups?**  
Los bind mounts son más sencillos de utilizar manualmente, basta con copiar la carpeta o usar un comando `tar`. Los volúmenes requieren comandos específicos de Docker.

---

## 2. Seguridad

**¿Es seguro tener las contraseñas en el archivo Compose?**  
No. Es una mala práctica si el archivo se sube a un repositorio público.

**¿Cómo mejoraría la seguridad usando `.env`?**  
Movería todas las variables sensibles al archivo `.env` y las referenciaría en Compose con `${VARIABLE}`. Además, añadiría `.env` al `.gitignore` para que nunca se suba al control de versiones.

**¿Qué otras medidas aplicaría?**
- Evitar usar el usuario root dentro de los contenedores.
- Limitar recursos (CPU/RAM) para prevenir ataques de denegación de servicio.
- Definir redes internas para la base de datos, de forma que solo sea accesible desde WordPress.

---

## 3. Persistencia

**¿Qué pasa si pierdo el volumen de WordPress?**  
Se perderían los archivos subidos, pero el contenido textual seguiría en la base de datos.

**¿Y si pierdo el volumen de MariaDB?**  
Se perdería toda la información del sitio: usuarios, contraseñas, posts, configuraciones y comentarios. WordPress volvería a su estado inicial de instalación.

**¿Cuál es más crítico?**  
El volumen de MariaDB. Los archivos de WordPress se pueden reinstalar, pero la base de datos contiene información única que no se recupera sin un backup.

---

## 4. Dependencias

**¿Por qué WordPress depende de la base de datos?**  
Porque guarda todo su estado. Sin ella, no puede mostrar nada al usuario.

**¿Qué pasa si arranco WordPress sin MariaDB?**  
El contenedor se inicia, pero la aplicación falla al intentar conectar y muestra error.

**¿`depends_on` garantiza que MariaDB esté lista?**  
No. Solo asegura que el contenedor de la base de datos se arranque antes que WordPress. Para eso se necesitan healthchecks con la condición `service_healthy`.

---

## 5. Comparación con práctica anterior

**¿Cuántos comandos necesitaba en la Práctica 2.3?**  
Minimo 4 para: crear la red, levantar la base de datos con sus variables, levantar WordPress con sus parámetros y gestionar volúmenes manualmente.

**¿Cuántos comandos necesito con Docker Compose?**  
Básicamente dos:
- `docker compose up -d` para arrancar.
- `docker compose down` para limpiar.

**¿Qué es más fácil de mantener?**  
Docker Compose. Toda la configuración está en un archivo YAML que se puede versionar y compartir, en lugar de depender de recordar comandos largos.

---

# Tarea 5.2: Escenarios de uso

Describe cómo usarías cada enfoque en estos escenarios:

### 1. Desarrollo local

**Elección:** Bind Mounts  
**Razón:** Necesito rapidez. Si cambio código CSS o PHP en mi editor, quiero ver el cambio inmediato en el navegador. Con volúmenes sería más lento porque tendría que copiar archivos dentro del contenedor.

---

### 2. Producción

**Elección:** Volúmenes  
**Razón:** En producción importa el rendimiento, la seguridad y la portabilidad. El código no se edita en vivo, debe ser inmutable. Los volúmenes gestionados por Docker son más rápidos y seguros en servidores Linux.

---

### 3. Testing / CI

**Elección:** Volúmenes
**Razón:** En pruebas automatizadas quiero un entorno limpio cada vez. Se levanta, se ejecutan los tests y se destruye. No necesito persistencia ni acceso desde el host, así que los volúmenes son lo más eficiente.