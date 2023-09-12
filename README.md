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

### 1.6 Optimizar Comentarios y Avatares

Los sistemas de cache de página normalmente cachean cada cierto tiempo la página y muestran una página estática. El problema viene si tenemos muchos visitantes dejando comentarios continuamente, ya que no podrán ver los últimos comentarios que han dejado otras visitas.

Es preferible que los usuarios no vean los comentarios más recientes durante un tiempo antes que sobrecargar el servidor borrando y regenerando el cache una y otra vez, ya que esto es un problema importante para el WPO.  La principal desventaja de configurar el cache para que se vacíe con cada comentario es que puede inutilizar por completo el sistema de comentarios y hacer que no sirva para nada, sobre todo cuanto más tráfico tengamos 
   
Para que los avatares carguen más rápido podemos implementar lazy load en los avatares, ya que los avatares al fin y al cabo son imágenes. Podemos implementar lazy load con librería Javascript o lazy load nativo, es recomendable activar los dos. Además de forma nativa, cada avatar que se carga desde Gravatar requiere una petición a un servidor externo y estas peticiones externas afectan negativamente al WPO porque no se pueden optimizar casi nunca. Si queremos hacer cache de avatares de Gravatar y que esas imágenes se guarden un tiempo en nuestro hosting para cargarse desde ahí, podemos hacerlo con plugins como LiteSpeed Cache o con el plugin FV Gravatar Cache.

Lazy load de los comentarios: Podemos llevar un poco más allá la optimización de la carga de los comentarios en WordPress con lazy load y que los comentarios carguen cuando pulsamos un botón. Para cargar los comentarios con un botón podemos usar Lazy Load for Comments. Esta opción puede afectar negativamente al SEO, ya que Google puede utilizar los comentarios de tu blog para determinar ciertos factores relacionados con el contenido fresco y la relevancia.

La otra opción es Paginar los Comentarios: Esto evitará que se carguen todos al mismo tiempo. WordPress, en su configuración, trae la opción de configuración necesaria para paginar los comentarios. Esta opción permititrá a GoogleBot seguir viendo los comentarios y la carga no se verá ralentizada aunque tengamos muchos.

### 1.7 Optimizar CRON WordPress

- Desactivar WP-Cron y activar un cron de sistema: se puede desactivar el cron de WordPress (WP-Cron) y crear un cron de sistema real. Esto suele ser una táctica de optimización habitual en sistemas sobrecargados, cuando es posible, y los requisitos de la web no lo impiden.

- Evitar que un mismo proceso se ejecute con demasiada frecuencia: Si lo que queremos es impedir que un mismo proceso se ejecute demasiado frecuentemente, podemos definir cuánto tiempo debe esperar cada proceso cron de WordPress para poder volver a ejecutarse.
Se consigue añadiendo una línea como la siguiente al archivo wp-config.php:

 ```
/* Definir frecuencia procesos cron 120 segundos */
define( 'WP_CRON_LOCK_TIMEOUT', 120 );

 ```
Crear intervalos personalizados de ejecución de WP-Cron: Lo primero que debemos hacer es elegir un intervalo en el que WP-Cron repetirá la tarea. Por defecto, los intervalos de WordPress son hourly (cada hora), twicedaily (dos veces al día), daily (cada día) y weekly (semanalmente). Y aunque cubre un espectro bastante amplio, puede que necesitemos un intervalo personalizado, que podemos añadir con un código como el siguiente:

```
/* Nuevo intervalo personalizado de wp-cron */
add_filter('cron_schedules', function ($schedules) {
return array_merge($schedules, [
// este es el nombre del intervalo personalizado, a usar cuando se cree una nueva tarea
'five_hours' => [
// el siguiente numero representa segundos, en este ejemplo: 5 hours * 60 minutes * 60 seconds
'interval' => 5 * 60 * 60,
'display' => esc_html__('Cada cinco horas'),
],
]);
});

```
Para añadir añadir una nueva tarea de WP-Cron tenemos que añadir un nuevo shortcode y añadir una programación al shortcode. El intervalo (el segundo argumento de la función wp_schedule_event()) debe ser uno de los intervalos por defecto de WordPress, o el del nombre personalizado que añadiste usando el código anterior.

```
/* Tarea cron usando intervalo personalizado de cada cinco horas */
add_action('YOUR_NAME_HERE', function () {
/* Aquí va la tarea cron
Puedes usar cualquier función de WordPress
p.ej. wp_insert_post() */
});
// A continuación comprobamos que el evento no estaba programado, ya que cada llamada crea uno nuevo
// en caso contrario, si no hacemos la comprobación, se creará un nuevo evento en cada petición de usuario
if (!wp_next_scheduled('PONLE_NOMBRE')) {
/* El primer argumento es la marca temporal que controla cuando 
se ejecutará por primera vez la tarea
el siguiente argumento refleja el intervalo elegido */
wp_schedule_event(time(), 'five_hours', 'PONLE_NOMBRE');
return;
}

```

El ejemplo anterior añade una tarea que ejecuta el cron de manera recurrente. Pero también podemos programar una tarea única. Para conseguirlo tendríamos que usar la función wp_schedule_single_event() en vez de wp_schedule_event(). Esta función acepta 2 tipos de argumentos: marca temporal con el momento de la siguiente ejecución, y el gancho que contiene la tarea. Con este ejemplo crearíamos una tarea que se ejecutará una sola vez, dentro de 5 horas:

```
/* Ejecutar tarea cron una sola vez dentro de cinco horas */
add_action('PONLE_NOMBRE', function () {
/* Aquí va la tarea cron
Puedes usar cualquier función de WordPress
p.ej. wp_insert_post() */
});
// A continuación comprobamos que el evento no estaba programado, ya que cada llamada crea uno nuevo
// en caso contrario, si no hacemos la comprobación, se creará un nuevo evento en cada petición de usuario
if (!wp_next_scheduled('PONLE_NOMBRE')) {
// Como antes, El primer argumento es la marca temporal que controla cuando se ejecuta la tarea
// se muestra en segundos y la fórmula es: 5 hours * 60 minutes * 60 seconds
wp_schedule_single_event(time() + 5 * 60 * 60, 'PONLE_NOMBRE');
return;
}

```

