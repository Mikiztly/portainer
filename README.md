# Portainer [![GitHub last commit](https://img.shields.io/github/last-commit/Mikiztly/portainer)](https://img.shields.io/github/last-commit/Mikiztly/portainer) [![GitHub](https://img.shields.io/github/license/Mikiztly/portainer)](https://img.shields.io/github/license/Mikiztly/portainer)

Son dos archivos para panejar un servidor Docker con [Portainer](https://www.portainer.io/), uno con Portainer solamente y una configuracion simple para tener solo ese servicio. El segundo le agrego [Nginx-Proxy-Manager](https://nginxproxymanager.com/) y [MariaDB](https://mariadb.com/) para poder manejar distintos dominios y sub-dominios.
Cabe aclarar que esta todo configurado para servidores linux, si se va a utilizar en Windows se deben cambiar las rutas.

# portainer.yml

Se puede descargar en la consola: <br>
```shell
wget -O docker-compose.yml https://github.com/Mikiztly/portainer/raw/main/portainer.yml
```
Con esta configuracion podemos manejar un servidor, podemos utilizar la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux) o si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux).
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.
Para ingresar al servicio se debe ingresar en el browser la direccion https://ip-servidor:9443/

# administracion.yml

Se puede utilizar en la consola:<br>

```shell
wget -O docker-compose.yml https://github.com/Mikiztly/portainer/raw/main/administracion.yml
```

Son cuatro servicios que se deben iniciar juntos para que funcionen bien: Portainer + MariaDB + phpMyAdmin + Nginx-Proxy-Manager. Estan configurados con IP estatica para poder conectarse con otros docks y por nombre de host, hay que tener cuidado con varios aspectos muy importantes:<br>
1) la declaracion de IP es manual para poder conectarse desde otros servicios, para agregar un nuevo servicio se debe declarar la IP y configurar la lan como "external: true".<br>
2) Los puertos no se declaran ya que se manejan con NPM (Nginx-Proxy-Manager).<br>
3) A todos los dock se les tiene que poner un nombre para poder referirlos en NPM.<br>
4) Se declaran los volumenes para redireccionar los datos a otro directorio/disco para tener persistencia de datos.<br>

Al correr este stack se van a crear los siguientes contenedores:

[Portainer](https://www.portainer.io/)<br>
Con esta configuracion podemos manejar un servidor, podemos utilizar la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux) o si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux).
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.

[Nginx-Proxy-Manager](https://nginxproxymanager.com/)<br>
Segun la documentacion oficial sirve para proporcionar a los usuarios una manera fácil de configurar hosts con un proxy inverso y certificados SSL, tiene que ser tan fácil que un mono pueda hacerlo. En resumen sirve para manejar dominios, sub-dominios, certificados ssl, etc.

[MariaDB](https://mariadb.com/)<br>
Es la Base de Datos mas popular y de codigo abierto, en principio es para tener la BD de NPM pero se pueden agregar mas Bases para otros proyectos como wordpress, etc.
Para poder utilizar algun software para manejar las BD se debe declarar el puerto 3306.

[phpMyAdmin](https://www.phpmyadmin.net/)<br>
Software para manjar base de datos de codigo abierto muy buena para manejar Base de Datos en el mismo servidor.

**IMPORTANTE**<br>
Como le agregue el mapeo de volumenes a otro directorio para tener persistencia de datos, utilizo la variable $HOME para referirme al directorio home del usuario. La ruta queda definida asi **$HOME/docker-data/[contenedor]**, si los datos se guardaran en otro directorio se debe poner la ruta completa.
Ademas en el directorio configurado tienen que existir las liguientes carpetas antes de levantar el stack:

letsencrypt<br> 
mariadb-data<br>
npm-data<br>
portainer-data<br>

Si no existen en el directorio (en mi caso /mnt/docker-data) va a dar error al levantar el stack.

Para ingresar y empezar a configurar NPM ingresamos a htt://ip-servidor:81/ una vez configurado un sub-dominio para NPM se puede cerrar ese puerto y dejar solamente los puertos HTTP (80) y HTTPS (443).

Agregue un explorador de archivos llamado [File Browser](https://filebrowser.org/) ya que vamos a necesitar crear carpetas y modificar bastante los archivos de configuracion esta opcion nos da acceso a los archivos de configuracion del servidor sin utilizar ftp, ssh, etc.

# portainer-swarm.yml
Se puede descargar en la consola: <br>

```shell
wget https://github.com/Mikiztly/portainer/raw/main/portainer-swarm.yml
```
Con esta configuracion podemos manejar hasta 3 nodos, con la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux). Si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux) se activan todas las opciones, sigue la restricción de 3 nodos con la licencia gratuita pero si queremos agregar mas nodos hay que pagar.
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.<br>
Para desplegar el stack ejecutamos lo siguiente:

```shell
docker stack deploy --compose-file=portainer-swarm.yml portainer
```

Para ingresar al servicio se debe poner el en browser la direccion https://ip-servidor:9443/

# administracion.swarm.yml

Se puede utilizar en la consola:<br>

```shell
wget https://github.com/Mikiztly/portainer/raw/main/administracion-swarm.yml
```

Esta configurado para funcionar en docker swarm y despliega cuatro servicios que se deben iniciar juntos para que funcionen bien: Portainer + MariaDB + phpMyAdmin + Nginx-Proxy-Manager. Estan configurados con IP estatica para poder conectarse con otros docks y por nombre de host, hay que tener cuidado con varios aspectos muy importantes:<br>
1) la declaracion de IP es manual para poder conectarse desde otros servicios, para agregar un nuevo servicio se debe declarar la IP y configurar la lan como "external: true".<br>
2) Los puertos no se declaran ya que se manejan con NPM (Nginx-Proxy-Manager).<br>
3) A todos los dock se les tiene que poner un nombre para poder referirlos en NPM.<br>
4) Se declaran los volumenes para redireccionar los datos a otro directorio/disco para tener persistencia de datos.<br>

Para desplegar el stack ejecutamos lo siguiente:

```shell
docker stack deploy --compose-file=administracion-swarm.yml portainer
```

Al correr este stack se van a crear los siguientes contenedores:

[Portainer](https://www.portainer.io/)<br>
Con esta configuracion podemos manejar un servidor, podemos utilizar la version [Community Edition](https://docs.portainer.io/start/install-ce/server/swarm/linux) o si te registras con [Business Edition](https://docs.portainer.io/start/install/server/swarm/linux).
Es una interfaz para manejar docker desde una web muy completa y facil de utilizar, tiene una configuracion basica para ver si nos convence, si se va a utilizar para produccion se deben hacer muchos cambios.

[Nginx-Proxy-Manager](https://nginxproxymanager.com/)<br>
Segun la documentacion oficial sirve para proporcionar a los usuarios una manera fácil de configurar hosts con un proxy inverso y certificados SSL, tiene que ser tan fácil que un mono pueda hacerlo. En resumen sirve para manejar dominios, sub-dominios, certificados ssl, etc.

[MariaDB](https://mariadb.com/)<br>
Es la Base de Datos mas popular y de codigo abierto, en principio es para tener la BD de NPM pero se pueden agregar mas Bases para otros proyectos como wordpress, etc.
Para poder utilizar algun software para manejar las BD se debe declarar el puerto 3306.

[phpMyAdmin](https://www.phpmyadmin.net/)<br>
Software para manjar base de datos de codigo abierto muy buena para manejar Base de Datos en el mismo servidor.

**IMPORTANTE**<br>
Al ser un cluster de servidores se tiene un almacenamiento compartido, se debe poner la ruta de ese almacenamiento. La ruta queda definida asi **/mnt/docker-data/[contenedor]**, si los datos se guardaran en otro directorio se debe poner la ruta completa.
Ademas en el directorio configurado tienen que existir las liguientes carpetas antes de levantar el stack:

letsencrypt<br> 
mariadb-data<br>
npm-data<br>
portainer-data<br>

Si no existen en el directorio (en mi caso /mnt/docker-data) va a dar error al levantar el stack.

Para ingresar y empezar a configurar NPM ingresamos a htt://ip-servidor:81/ una vez configurado un sub-dominio para NPM se puede cerrar ese puerto y dejar solamente los puertos HTTP (80) y HTTPS (443).

Agregue un explorador de archivos llamado [File Browser](https://filebrowser.org/) ya que vamos a necesitar crear carpetas y modificar bastante los archivos de configuracion esta opcion nos da acceso a los archivos de configuracion del servidor sin utilizar ftp, ssh, etc.
