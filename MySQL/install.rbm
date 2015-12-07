#Instalar Apache, PHP, MySQL y phpMyAdmin en Ubuntu 11.04
Lo siguiente es la instalación más básica y menos segura de dichos programas, no recomendado para un servidor en producción.

Abrir una terminal/consola, primero instalamos el servidor web Apache:

  sudo apt-get install apache2

Lo podemos comprobar abriendo un navegador en la dirección:

  http://127.0.0.1

Cambiar la ruta /var/www/html a /var/www para ello:
  sudo vim /etc/apache2/sites-available/000-default.conf
  
Y modificar la ruta en la linea:
  DocumentRoot /var/www/html
  
Instalamos el PHP 5 cómo módulo:

  sudo apt-get install php5 libapache2-mod-php5

Reiniciamos el servidor web:

  sudo /etc/init.d/apache2 restart

El directorio www por default es:

/var/www

Si no tenemos permiso para manipular su contenido, se lo damos con lo siguiente. Cambiamos el propietario del directorio y el grupo que debe usarlo. Reemplazar USUARIO con el nombre de usuario que estén utilizando:

  sudo chown -R USUARIO:www-data /var/www

Se le dan permisos de lectura y ejecución para todos y de escritura sólo al propietario:

  sudo chmod -R 755 /var/www

Ahora creamos el info.php de rigor para comprobar el funcionamiento de PHP:

  sudo gedit /var/www/info.php

Pegar lo siguiente dentro, luego salvar y cerrar:

  <?php phpinfo(); ?>

Comprobar entrando a la dirección:

  http://127.0.0.1/info.php

Tendría que aparecer toda la información de configuración del PHP y sus módulos instalados. Seguimos con la instalación del servidor y el cliente de MySQL

  sudo apt-get install mysql-server mysql-client

Pedirá clave para el usuario root y luego la confirmación de la misma. Ahora podemos instalar todos estos módulos, mejor que sobre y no que falte :P

  sudo apt-get install php5-mysql php5-curl php5-gd php5-idn php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

Entre ellos va el soporte para MySQL, cURL, etc. Ahora otro reinicio del servidor web:

  sudo /etc/init.d/apache2 restart

Y ahora instalamos la interfaz web para manejar el MySQL y sus bases de datos, phpMyAdmin:

  sudo apt-get install phpmyadmin

Preguntará para que servidor web configurar, elegir apache2 y continuar. Luego pedirá configurar la base de datos con dbconfig-common elegir que No.

Comprobar si funciona entrando a:

  http://127.0.0.1/phpmyadmin

------------------------
Si no funciona, ejecutar:

  sudo gedit /etc/apache2/httpd.conf

Pegar lo siguiente dentro, luego salvar y cerrar:

  Include /etc/phpmyadmin/apache.conf

Reiniciar el servidor web nuevamente:

  sudo /etc/init.d/apache2 restart

Y con eso ya debería estar todo funcionando.

Archivos y rutas importantes:

* acá están todos los virtual hosts habilitados
  /etc/apache2/sites-enabled

* el virtual host por default, de este se pueden hacer copias
  /etc/apache2/sites-available/default

* el archivo de configuración de PHP
  /etc/php5/apache2/php.ini

* el archivo de configuración global de MySQL
  /etc/mysql/my.cnf
  
  fuente: [aquí https://edrperez.wordpress.com/2011/07/18/instalar-apache-php-mysql-y-phpmyadmin-en-ubuntu-11-04/]
