# Practica servidor web  
## 1. Titulo  
Wordpress con docker compose YML

---

## 2. Tiempo de duración  
**60 minutos**.

---

## 3. Fundamentos  

ChatGPT Plus

Los fundamentos de esta práctica se basan en la comprensión del uso de contenedores mediante Docker para desplegar aplicaciones de forma aislada, eficiente y reproducible. A través de Docker Compose se orquestan múltiples servicios interdependientes, como WordPress, MySQL y phpMyAdmin, permitiendo su configuración conjunta dentro de un mismo entorno. Además, se aplica el concepto de redes internas para la comunicación segura entre servicios y volúmenes persistentes para garantizar que los datos de la base de datos no se pierdan al reiniciar los contenedores. Esta estructura refleja un entorno real de desarrollo web moderno, donde se combinan un CMS, un gestor de bases de datos y una herramienta de administración, todo bajo un sistema de contenedores estandarizado.
## 4. Conocimientos previos  

- Manejo básico de la terminal de Linux o CMD/PowerShell. 
- Conocimientos iniciales sobre Docker: imágenes, contenedores, volúmenes y redes.
- Familiaridad con conceptos de bases de datos (tablas, usuarios, permisos).
- Nociones sobre archivos YAML y su estructura. 


## 5. Objetivos a alcanzar  

- Construir un archivo docker-compose.yml funcional. 
- Desplegar tres servicios: WordPress, MySQL y phpMyAdmin.
- Crear y utilizar una red personalizada para la interconexión de los servicios.
- Implementar un volumen persistente para evitar pérdida de datos.
- Acceder y gestionar la base de datos desde phpMyAdmin.

---

## 6. Equipo necesario:  

- Computadora de escritorio o portátil con mínimo:
- Conexión a Internet para descargar imágenes de Docker.
- Sistema operativo compatible.


---

## 7. Material de apoyo.

- Documentación oficial de Docker.
- Documentación de WordPress  
- Guías de uso de phpMyAdmin.
- Guía de la asignatura  
- Editor de texto para crear archivos YAML (VSCode recomendado).

---

## 8. Procedimiento

### Paso 1: Crear la carpeta del proyecto  
Crear la carpeta:
```
TA5-Wordpress-Compose
```

### Paso 2: Crear el archivo `docker-compose.yml`  
Copiar el siguiente contenido:

```yml
services:
  db:
    image: mysql:8.0
    container_name: mysql-wordpress
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wp_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_pass
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wp_net

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin-wordpress
    restart: always
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: rootpass
    ports:
      - "8080:80"
    depends_on:
      - db
    networks:
      - wp_net

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    restart: always
    ports:
      - "8081:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_pass
      WORDPRESS_DB_NAME: wp_db
    depends_on:
      - db
    networks:
      - wp_net

volumes:
  db_data:

networks:
  wp_net:
```

<p align="center">
  <img src="archivo" width="800px">
</p>
<p align="center"><i>Figura 2-1. Archivo docker-compose.yml.</i></p>

---

### Paso 3: Levantar los contenedores

```
docker compose up -d
```

<p align="center">
  <img src="verificar" width="800px">
</p>
<p align="center"><i>Figura 3-1. Ejecución del comando docker compose up.</i></p>

---

### Paso 4: Verificar el estado

```
docker compose ps
```

### Paso 5: Acceder a phpMyAdmin  

Abrir:  
**http://localhost:8080**

<p align="center">
  <img src="php" width="800px">
</p>
<p align="center"><i>Figura 5-1. phpMyAdmin.</i></p>

---

### Paso 6: Acceder a WordPress  

Abrir:  
**http://localhost:8081**

<p align="center">
  <img src="wordpress" width="800px">
</p>
<p align="center"><i>Figura 6-1. Instalación inicial de WordPress.</i></p>

---

## 9. Resultados esperados  

- Levantar correctamente el entorno WordPress–MySQL–phpMyAdmin usando Docker Compose. 
- Acceder a WordPress mediante http://localhost:8080.
- Acceder a phpMyAdmin mediante http://localhost:8081.
- Comprender el funcionamiento modular y escalable de un entorno web mediante Docker.


## 10. Bibliografía  

- Docker Inc. (2024). *Docker Documentation*. https://docs.docker.com  
- WordPress.org. WordPress Documentation. https://wordpress.org/support
- MySQL. MySQL Reference Manual. https://dev.mysql.com/doc