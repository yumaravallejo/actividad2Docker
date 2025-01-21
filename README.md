# Actividad 2. Configuración de Proxy Inverso

Este es un proyecto educativo realizado por Yumara Vallejo para el tema 3 que trata sobre Servidores de aplicaciones, en concreto es iniciación sobre Docker. Concretamente, en este proyecto se han realizado 3 páginas que alojaremos en 3 contenedores dockers con su respectivo dockerfile.

## Tabla de Contenidos

- [Creación de imágenes, contenedores y configuración de Docker](#docker)
- [Integración de Apache con Docker](#apache_docker)
- [Comprobaciones](#comprobaciones)
- [Pruebas de rendimiento](#pruebas_de_rendimiento)

## Preparación
Antes de comenzar, se necesita tener docker y docker compose instalados, además de apache para poder configurar el proxy inverso. Para obtener los archivos base puede clonar o descargar este repositorio.

## Creación de imágenes, contenedores y configuración de Docker
Con los archivos instalados podremos dirigirnos hacia cada una de las rutas con su Dockerfile y generar la imagen de cada web usando:
```bash
  sudo docker build -t auladaw .
  sudo docker build -t bicicleta .
  sudo docker build -t coches .
```
Tras crear las imágenes crearemos los contenedores en su puerto asignado para esta práctica aunque puedes seleccionar el que quieras siempre y cuando cambies los archivos necesarios (si no tienes mucha idea dejar como está)
```bash
  sudo docker run -d -p 8080:80 –name cnt_auladaw auladaw
  sudo docker run -d -p 8081:80 –name cnt_bicicleta bicicleta
  sudo docker run -d -p 8082:80 –name cnt_coches coches
```

## Integración de Apache con Docker
En este punto será donde necesitemos usar los módulos de proxy y proxy_http por lo que deberás activarlos si no los tienes y reiniciar apache, además para la web de coches necesitaremos también el módulo ssl activado. Crea un certificado y clave para la web de coches usando el siguiente comando (es recomendado no cambiar el nombre para no tener que tocar demás archivos):
```bash
 sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/cocheses-selfsigned.key -out etc/ssl/certs/cocheses-selfsigned.crt
```
Ahora deberás añadir los archivos .conf a la carpeta de /etc/apache2/sites-available para así poder activar cada uno de ellos usando:
```bash
 a2ensite www.aula2daw.com.conf
 a2ensite www.bicicletas.com.conf
 a2ensite www.coches.com.conf
```
Cambiar el archivo hosts por el de tu máquina o simplemente añadir las rutas al tuyo, al igual que con el archivo ports que deberás poner Listen 8084 (o uno que sepas que no dará conflicto) en lugar de la 8080.

## Comprobaciones
Para las comprobaciones solo tendrás que poner la ruta que has puesto en tu archivo hosts en tu navegador, cuidado con los http y los https.

## Pruebas de rendimiento
Para las pruebas de rendimiento haremos uso de Apache Bench, si no lo tienes instalado deberás hacerlo con el comando:
```bash
 sudo apt install apache2-utils 
```
Tras instalarlo solo necesitaremos poner por cada una de nuestras páginas lo siguiente:
```bash
  ab -n 1000 -c 10 http://www.aula2daw.es/
  ab -n 1000 -c 10 http://www.bicicletas.es/
  ab -n 1000 -c 10 http://www.coches.es/
```
