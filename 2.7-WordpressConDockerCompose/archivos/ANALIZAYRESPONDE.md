
### ¿Por qué hay dos volúmenes diferentes?

Porque los datos son de naturaleza distinta y los gestionan servicios distintos. `db_data` guarda las tablas SQL y `wp_data` guarda archivos estáticos.

### ¿Qué datos almacena cada volumen?

`db_data contiene` la base de datos MariaDB pura. `wp_data` contiene la carpeta wp-content.

### ¿Por qué WordPress usa el nombre del servicio de base de datos como hostname?

Docker crea un DNS interno. Cuando WordPress intenta conectar a "db", Docker resuelve ese nombre a la IP interna del contenedor de MariaDB automáticamente.
