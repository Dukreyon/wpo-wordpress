# wpo-wordpress
Guia WPO Wordpress

Guia de recursos para optimizar la velocidad de carga de WordPress

## 1 Optimizar Configuración Wordpress

### 1.1 Modificar el wp-config.php 
- Revisiones de Posts: Para limitar el número de revisiones que WP guarda a tres por entrada, introduzca la siguiente línea en wp-config.php:
  
```
define( ‘WP_POST_REVISIONS’, 3 );

```

Si prefiere no almacenar ninguna revisión, puede utilizar la siguiente línea:

```
define( ‘WP_POST_REVISIONS’, false );
```

- Intervalo de autoguardado de WP: Si quieres que WordPress guarde automáticamente tu trabajo cada 180 segundos, necesitas la siguiente línea en el archivo wp-config.php:
  
```
  define(‘AUTOSAVE_INTERVAL’, 180 );
```

- Definición de la URL de inicio y del sitio: cada vez que un plugin o un tema necesita acceder a ellos, envía una consulta SQL. Esto ocurre más a menudo de lo que crees, y procesar todas estas consultas puede acabar consumiendo una cantidad significativa de recursos de hardware. Definir las URL de inicio y del sitio en el archivo wp-config.php reducirá la carga de la base de datos y mejorará el rendimiento. Esto es lo que tienes que añadir:

```
  define(‘WP_HOME’, ‘http://www.yourdomain.com’); 
  define(‘WP_SITEURL’, ‘http://www.yourdomain.com’);
```

- Vaciar la papelera WP con más frecuencia
```
  define(‘EMPTY_TRASH_DAYS’, 7 );
```

- Compresión archivos:

  ```
  define( 'COMPRESS_CSS',        true );
  define( 'COMPRESS_SCRIPTS',    true );
  define( 'CONCATENATE_SCRIPTS', true );
  define( 'ENFORCE_GZIP',        true );

  ```

### 1.2 Modificar el .htaccess 

(Fuente https://alvarofontela.com/htaccess-para-wordpress-tips-trucos/)

- Activar la compresión GZIP / DEFLATE

```
    <ifModule mod_gzip.c>
      mod_gzip_on Yes
      mod_gzip_dechunk Yes
      mod_gzip_item_include file .(html?|txt|css|js|php|pl)$
      mod_gzip_item_include handler ^cgi-script$
      mod_gzip_item_include mime ^text/.*
      mod_gzip_item_include mime ^application/x-javascript.*
      mod_gzip_item_exclude mime ^image/.*
      mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
    </ifModule>
```

- Activar Keep Alive en Apache: El parámetro Keep Alive permite que la conexión entre el servidor y el navegador del visitante no se cierre. Como resultado, si hay que realizar varias peticiones consecutivas, al mantener la conexión abierta se realizarán mucho más rápidamente. Es lo que comúnmente se llama “conexión persistente”.

 
```
  #Activar Keep Alive
  KeepAlive On
 
  #Conexiones maximas persistentes por Keep Alive
  MaxKeepAliveRequests 100
 
  #Keep Alive Timeout de cada conexion
  KeepAliveTimeout 100

```
- Activar cache de navegador o browser cache: El browser cache nos permite ahorrar recursos en el servidor web y también ancho de banda en el hosting o servidor.
  
```
 <IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/gif "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/webp "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
  ExpiresByType image/x-icon "access plus 1 year"
  ExpiresByType video/mp4 "access plus 1 year"
  ExpiresByType video/mpeg "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType text/javascript "access plus 1 month"
  ExpiresByType application/javascript "access plus 1 month"
  ExpiresByType application/pdf "access plus 1 month"
  ExpiresByType application/x-shockwave-flash "access plus 1 month"
</IfModule>

```
### 1.3 Borrar contenido por defecto de WordPress

Wordpress viene por defecto con temas, plugins y widgets que no se utilizan. 

- Eliminar plugins que no usarás:

   -- Hello Dolly

- Borrar themes de WordPress que no utilizas:

   -- Borrar los temas que no tengas activos

- Borrar post de WordPress: Hellow World

### 1.4 Optimizar la base de datos de WordPress

Para optimizar la base de datos podmeos utilizar el plugin [WP-Sweep](https://wordpress.org/plugins/wp-sweep/)

- 1.4.1 Elimina las revisiones, borradores automáticos y entradas descartadas
  
- 1.4.2 Elimina comentarios spam y descartados 

- 1.4.3 Elimina transients o datos transitorios 

- 1.4.4 Desactiva y elimina Pingbacks y Trackbacks 

- 1.4.5 Limpia la base de datos de WordPress 

- 1.4.6 Utiliza Memcached en WordPress

- 1.4.7 Elimina queries lentas de la base de datos de WordPress 

- 1.4.8 Optimiza las tablas de la base de datos 

- 1.4.9 Activa la limpieza automática 

- 1.4.10 Optimizar la base de datos de WordPress desde PHPMyadmin
  
  PhpMyAdmin nos permite optimizar las tablas desde el menú desplegable principal. Lo que podemos hacer para optimizar la base de datos de WordPress es hacer click en la casilla “Seleccionar todo” y  seleccionar “Optimizar la tabla” en el menú desplegable. PhpMyAdmin lo hará todo automáticamente.

### 1.5 Arreglar contenido Mixto en WordPress

Esto suele ser debido a 2 motivos:

1) Has instalado un certificado SSL válido pero tu web aún es accesible sin HTTPS porque no has realizado la redirección de HTTP a HTTPS.
2) Has instalado un certificado SSL válido, has hecho la redirección, pero tu web sirve contenido mixto, bajo HTTP.

   Para solucionarlo podemos utilizar el plugin [Really Simple SSL](https://es.wordpress.org/plugins/really-simple-ssl/). Simplemente activa el plugin y pulsa el botón para activar el SSL.

   También podemos realizarlo mediante el archivo .htaccess con estas instrucciones

   ```
     define('FORCE_SSL_LOGIN', true);
     define('FORCE_SSL_ADMIN', true);
   
   ```
   
   


