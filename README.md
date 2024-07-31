# Portainer [![GitHub last commit](https://img.shields.io/github/last-commit/Mikiztly/portainer)](https://img.shields.io/github/last-commit/Mikiztly/portainer) [![GitHub](https://img.shields.io/github/license/Mikiztly/portainer)](https://img.shields.io/github/license/Mikiztly/portainer)

Son dos archivos para panejar un servidor Docker con [Portainer](https://www.portainer.io/), uno con Portainer solamente y una configuracion simple para tener solo ese servicio. El segundo le agrego [Nginx-Proxy-Manager](https://nginxproxymanager.com/) y [MariaDB](https://mariadb.com/) para poder manejar distintos dominios y sub-dominios.
Cabe aclarar que esta todo configurado para servidores linux, si se va a utilizar en Windows se deben cambiar las rutas.

**portainer.yml**

Se puede descargar en la consola: <br>
wget -O docker-compose.yml https://github.com/Mikiztly/portainer/raw/main/portainer.yml

Sirve para manejar hasta 3 nodos con la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux) o si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux), para mas nodos hay que pagar.
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.
Para ingresar al servicio se debe poner el en browser la direccion https://ip-servidor:9443/

**portainer+npm+mariadb.yml**

Se puede utilizar en la consola:<br>
wget -O docker-compose.yml https://github.com/Mikiztly/portainer/raw/main/portainer+npm+mariadb.yml

Son cuatro servicios que se deben iniciar juntos para que funcionen bien: Portainer + MariaDB + phpMyAdmin + Nginx-Proxy-Manager. Estan configurados con IP estatica para poder conectarse con otros docks y por nombre de host, hay que tener cuidado con varios aspectos muy importantes:<br>
1) la declaracion de IP es manual para poder conectarse desde otros servicios, para agregar un nuevo servicio se debe declarar la IP y configurar la lan como "external: true".<br>
2) Los puertos no se declaran ya que se manejan con NPM (Nginx-Proxy-Manager).<br>
3) A todos los dock se les tiene que poner un nombre para poder referirlos en NPM.<br>
4) Se declaran los volumenes para redireccionar los datos a otro directorio/disco para tener persistencia de datos.

[Portainer](https://www.portainer.io/)<br>
Sirve para manejar hasta 3 nodos con la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux) o si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux), para mas nodos hay que pagar.
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.

[Nginx-Proxy-Manager](https://nginxproxymanager.com/)<br>
Segun la documentacion oficial sirve para proporcionar a los usuarios una manera fácil de configurar hosts con un proxy inverso y certificados SSL, tiene que ser tan fácil que un mono pueda hacerlo. En resumen sirve para manejar dominios, sub-dominios, certificados ssl, etc.

[MariaDB](https://mariadb.com/)<br>
Es la Base de Datos mas popular y de codigo abierto, en principio es para tener la BD de NPM pero se pueden agregar mas Bases para otros proyectos como wordpress, etc.
Para poder utilizar algun software para manejar las BD se debe declarar el puerto 3306.

[phpMyAdmin](https://www.phpmyadmin.net/)<br>
Software para manjar base de datos de codigo abierto muy buena para manejar Base de Datos en el mismo servidor.

**IMPORTANTE**<br>
Como le agregue el mapeo de volumenes a otro directorio para tener persistencia de datos, en el directorio configurado tienen que existir las liguientes carpetas antes de levantar el stack:

portainer-data<br>
mariadb-data<br>
npm-data<br>
letsencrypt<br> 

Si no existen en el directorio (en mi caso /mnt/docker-data) va a dar error al levantar el stack.

Para ingresar y empezar a configurar NPM ingresamos a htt://ip-servidor:81/ una vez configurado un sub-dominio para NPM se buede cerrar ese puerto y dejar solamente los puertos HTTP (80) y HTTPS (443).
