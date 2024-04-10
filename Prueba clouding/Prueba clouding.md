
En este repositorio solo hay una copia de la BD y una imagen que se utiliza en un post. El ejercicio consite en crear un servidor Linux en blanco, no se pueden usar las imágenes de LEMP, LAMP, Wordpress o Ubuntu Desktop.

Básicamente hay que instalar un serviddor web con PHP y MySQL o MariaDB. Hay que instalar [Wordpress](https://wordpress.org/latest.zip) e importar la copia de la BD y las imágenes. Antes de importar la BD hay que editar el fichero testWP.sql buscar 46.183.115.141 y remplazarlo por la IP del servidor que hemos creado.

Datos de acceso al Admin de Wordpress:

- url: [http://ip-servidor/wp-admin](http://ip-servidor/wp-admin)
- usuario: WPadmin
- contraseña: 5IVMdKcQmE0SOxZi

Se puede consultar cualquier artículo de nuestra base de conocimientos. Seguramente haga falta configurar algún tema en el administrador de Wordpress.

Se valorará cambiar la IP de Worpress por el nombre aleatorio de clouding.host y la generación de un SSL con Let's Encrypt. No se valorará el tiempo invertido en hacer el ejercicio, se valorará el resultado final. No borréis el history del servidor.

Podeís crear snapshot a medida que vayáis progresando en el ejercicio, así podréis retroceder a cualqueir paso en caso de fallo.

Crear un servidor Linux en blanco.

Cambiar password:
passwd root qwe123$!


Instalar PHP
`Consultar si el php esta activdo
php -v
`Via apache web
/php_version_check.php

`Instalación de php
sudo apt update
sudo apt install -y libapache2-mod-php

Instalar MySQL (phpmyadmin)

```
sudo apt-get install mysql-server mysql-client
sudo service mysql start
sudo apt-get install phpmyadmin php-mbstring php-gettext
YES en el daemon
```

/usr/share/doc/phpmyadmin

define('AUTH_KEY',         'RI)U94frQ8=&A[bZXKXMLOX}R+O/02Wgv1)H|ZsBJPN@S,ToVmJ=8yza9rr~pPEU');
define('SECURE_AUTH_KEY',  '~ts,u{oPX]er+hY&;5x# b=bp`#AsAjc0<:SE:v,c4s/ 0%Ol;XQ[d*Q]|^Sadc+');
define('LOGGED_IN_KEY',    '-C_EdsIL->b{.A-J2A3xT&~=;KqW98w[.+UagS(*x$l&,vQPv+sc-qaIyz&)WsKh');
define('NONCE_KEY',        'Z4L#z`<QdA)$3FBwr2fvHQ!+e4kB=rjPt$+$F1M@9Q{<[%|h2|J9%>h[agQcl5W$');
define('AUTH_SALT',        '`*0pwb?3V1WfI,!:?<xP+RkNYC~!WmL^xh]|DqmRM7D:%W}QD<~HdX5~k>WXp}h:');
define('SECURE_AUTH_SALT', 'bHKAN@?xqP[Qud4NQVTkC,-XNEYRr7&cS54!z-6}J%RnqCA(E+W~4Q(=AgX_L+k;');
define('LOGGED_IN_SALT',   'fVLkjauT*?t6?CfJH&5{ S Y{@3)9EWKbQ-TKG53~++|+<~x;Pq>Zs)2OcHS+i{Q');
define('NONCE_SALT',       ']Dg_vY+pa~vtd19-{WzbS,~vV1ZZeZ!mH|U-ro?/KnBsJ,~=arEW_em}N:kv 4,t');