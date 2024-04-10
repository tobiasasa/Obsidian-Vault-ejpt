<?php
/**
 * The base configuration for WordPress
 *
 * The wp-config.php creation script uses this file during the installation.
 * You don't have to use the web site, you can copy this file to "wp-config.php"
 * and fill in the values.
 *
 * This file contains the following configurations:
 *
 * * Database settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/documentation/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'root' );

/** Database password */
define( 'DB_PASSWORD', 'qwerty123prueba' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication unique keys and salts.
 *
 * Change these to different unique phrases! You can generate these using
 * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
 *
 * You can change these at any point in time to invalidate all existing cookies.
 * This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define('AUTH_KEY',         'RI)U94frQ8=&A[bZXKXMLOX}R+O/02Wgv1)H|ZsBJPN@S,ToVmJ=8yza9rr~pPEU');
define('SECURE_AUTH_KEY',  '~ts,u{oPX]er+hY&;5x# b=bp`#AsAjc0<:SE:v,c4s/ 0%Ol;XQ[d*Q]|^Sadc+');
define('LOGGED_IN_KEY',    '-C_EdsIL->b{.A-J2A3xT&~=;KqW98w[.+UagS(*x$l&,vQPv+sc-qaIyz&)WsKh');
define('NONCE_KEY',        'Z4L#z`<QdA)$3FBwr2fvHQ!+e4kB=rjPt$+$F1M@9Q{<[%|h2|J9%>h[agQcl5W$');
define('AUTH_SALT',        '`*0pwb?3V1WfI,!:?<xP+RkNYC~!WmL^xh]|DqmRM7D:%W}QD<~HdX5~k>WXp}h:');
define('SECURE_AUTH_SALT', 'bHKAN@?xqP[Qud4NQVTkC,-XNEYRr7&cS54!z-6}J%RnqCA(E+W~4Q(=AgX_L+k;');
define('LOGGED_IN_SALT',   'fVLkjauT*?t6?CfJH&5{ S Y{@3)9EWKbQ-TKG53~++|+<~x;Pq>Zs)2OcHS+i{Q');
define('NONCE_SALT',       ']Dg_vY+pa~vtd19-{WzbS,~vV1ZZeZ!mH|U-ro?/KnBsJ,~=arEW_em}N:kv 4,t');

/**#@-*/

/**
 * WordPress database table prefix.
 *
 * You can have multiple installations in one database if you give each
 * a unique prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 *
 * For information on other constants that can be used for debugging,
 * visit the documentation.
 *
 * @link https://wordpress.org/documentation/article/debugging-in-wordpress/
 */
define( 'WP_DEBUG', false );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
        define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
define('FS_METHOD', 'direct');





