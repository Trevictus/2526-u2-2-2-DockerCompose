# Tarea 5.1: Preguntas de Análisis

Responde a las siguientes preguntas en tu documentación:

---

## 1. Docker Compose vs. Comandos Manuales

### ¿Qué ventajas ofrece Docker Compose frente a ejecutar comandos `docker run` manualmente?
- **Centralización:** Toda la configuración está en un solo archivo.
- **Orquestación automática:** Gestiona el orden de arranque.
- **Gestión del ciclo de vida simplificada:** Permite crear, iniciar, detener y destruir todo el entorno con un solo comando (`up` / `down`).
- **Persistencia de configuración:** No hace falta recordar flags complejos para arrancar el servicio.

### ¿En qué escenarios sería preferible usar comandos manuales?
- **Pruebas rápidas:** Para probar una imagen nueva rápidamente sin crear archivos.
- **Depuración:** Para ejecutar herramientas de diagnóstico temporalmente en una red existente.
- **Aprendizaje:** Para entender cómo Docker gestiona los recursos antes de usar Compose.

### ¿Cómo facilita Docker Compose el trabajo en equipo?
- Garantiza que el entorno sea reproducible.
- Al compartir el archivo `docker-compose.yml` en el repositorio del proyecto, todos los desarrolladores levantan exactamente la misma infraestructura con un solo comando.

---

## 2. Archivo `docker-compose.yml`

### ¿Por qué se considera "Infrastructure as Code"?
Porque la infraestructura necesaria para ejecutar la aplicación no se crea manualmente mediante clics o comandos interactivos, sino que se define en un archivo de texto.

### ¿Qué ventajas tiene definir la infraestructura de forma declarativa?
- En el modelo YAML, describes el estado final deseado y Docker se encarga de realizar las acciones necesarias para llegar a ese estado.
- No tienes que escribir el proceso paso a paso.
- Facilita la automatización.

### ¿Cómo se versionaría este archivo en un proyecto real?
- El archivo `docker-compose.yml` se incluye en el sistema de control de versiones.
- Permite ver cambios en la infraestructura.
- Crear ramas para probar nuevas configuraciones sin romper producción.
- Facilitar la revisión de código.

---

## 3. Redes en Docker Compose

### ¿Qué red se crea automáticamente si no se define una explícitamente?
Se crea una red de tipo **bridge** llamada por defecto `<nombre_carpeta_proyecto>_default`.

### ¿Cómo funcionan los nombres de servicio para la resolución DNS?
- Docker incorpora un servidor DNS interno.
- Dentro de la red creada por Compose, los contenedores pueden comunicarse entre sí utilizando el nombre del servicio definido en el YAML como si fuera un nombre de dominio.

### ¿Cuándo es necesario definir redes personalizadas?
- **Aislamiento de seguridad:** Separar servicios que no deben comunicarse entre sí.
- **Conexión externa:** Para conectar servicios de este proyecto con contenedores gestionados por otro `docker-compose.yml`.
- **Configuración específica:** Definir IPs estática.

---

## 4. Volúmenes Docker vs. Bind Mount

### ¿Qué ventajas tienen los volúmenes Docker sobre bind mounts?
- **Gestión:** Administrados enteramente por Docker.
- **Independencia:** Son más portables que los bind mounts.
- **Rendimiento:** En Docker Desktop (Windows/Mac), los volúmenes tienen mejor rendimiento.
- **Seguridad:** Menor riesgo de modificar archivos del sistema host accidentalmente.

### ¿Cuándo usarías bind mount en Docker Compose?
- Principalmente en entornos de desarrollo.
- Se usan para montar el código fuente local dentro del contenedor.

### ¿Cómo se gestionan los volúmenes con Docker Compose?
- **Top-level volumes:** Al final del archivo, para registrar que el volumen existe.
- **Service-level:** Dentro de cada servicio, indicando el punto de montaje.
- Compose crea el volumen si no existe (`up`) o mantenerlo si ya existe, garantizando la persistencia.

---

## 5. Escalabilidad

### ¿Por qué no se puede escalar el servicio `app` fácilmente?
- Debido al conflicto de puertos.
- En la configuración actual, se mapea el puerto fijo del host al contenedor (`5000:5000`).
- Si se intenta levantar una segunda instancia (`--scale app=2`), esta intentará ocupar también el puerto 5000 del host, lo cual es imposible.

### ¿Cómo se podría modificar el archivo para permitir escalado?

### ¿Se puede escalar el servicio de base de datos `db`?
- No, el servicio de base de datos `db` no se puede escalar fácilmente en este caso.
- Se debe crear un servicio independiente para cada instancia.

## 6. Políticas de reinicio

### ¿Qué significa `restart: always`?
- Indica que Docker siempre debe intentar reiniciar el contenedor si este se detiene, sin importar la razón.

### ¿Qué otras políticas de reinicio existen?
- **no:** Valor por defecto. No reinicia el contenedor bajo ninguna circunstancia.
- **on-failure:** Solo reinicia si el contenedor se detiene con un código de salida distinto de 0.
- **unless-stopped:** Como `always`, pero si el contenedor estaba detenido explícitamente antes de un reinicio del sistema, se mantendrá detenido.

### ¿En qué casos utilizarías cada una?
- **no:** Para tareas de un solo uso.
- **on-failure:** Para aplicaciones que deben reintentar si algo falla.
- **always / unless-stopped:** Para servicios web, bases de datos y servidores que deben estar siempre disponibles en producción.  