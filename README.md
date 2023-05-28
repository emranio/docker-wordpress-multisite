# wordpress environment on docker with mariadb 

- clone this git repo `git clone git@github.com:emranio/docker-wordpress-multisite.git`. Run the following command to run the wordpress environment on docker

    'docker compose up -d'

- to run all containers of the `docker-compose.yml` file perfectly,  make available your computer `80` ports.
- If you want to change the password of your database change it on environment section of `wp` services of `docker-compose-yml` file

    ```
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_NAME=test
      - WORDPRESS_DB_USER=root
      - WORDPRESS_DB_PASSWORD=test
      - WORDPRESS_LOCAL_DEV=true
      - WORDPRESS_DEBUG=true
    ```


- also on db environment section

    ```
    environment:
      - MYSQL_DATABASE=test
      - MYSQL_ROOT_PASSWORD=test
      - MYSQL_USER=root
    ```

- If you face `Error establishing a database connection`, delete your `wp-config.php` file and reload and install it again.

### Multi site configuration

- Go to `setting->permalinks` and set the permalink structure as `Post name` and save it.
- Open `wp-config.php` file and paste the constants `define( 'WP_ALLOW_MULTISITE', true );` after the `WP-DEBUG` section and save it.
- Now reload your wp-admin.
- Go to `Tools->Network Setup` choose sub directory options and fill the fields and click on `install` button.
- Copy the following code and paste it on `wp-config.php` file after `WP_ALLOW_MULTISITE` constant.

  ```
  define( 'MULTISITE', true );
  define( 'SUBDOMAIN_INSTALL', false );
  define( 'DOMAIN_CURRENT_SITE', 'localhost' );
  define( 'PATH_CURRENT_SITE', '/' );
  define( 'SITE_ID_CURRENT_SITE', 1 );
  define( 'BLOG_ID_CURRENT_SITE', 1 );
  ```

- Copy the following code and select all and paste it on `.htaccess` file which is on your wordpress root directory.

 ```
  RewriteEngine On
  RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
  RewriteBase /
  RewriteRule ^index\.php$ - [L]

  # add a trailing slash to /wp-admin
  RewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ $1wp-admin/ [R=301,L]

  RewriteCond %{REQUEST_FILENAME} -f [OR]
  RewriteCond %{REQUEST_FILENAME} -d
  RewriteRule ^ - [L]
  RewriteRule ^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) $2 [L]
  RewriteRule ^([_0-9a-zA-Z-]+/)?(.*\.php)$ $2 [L]
  RewriteRule . index.php [L]
  ```
- Now reload your admin panel you will see multi site menu on your left top bar beside the wordpress home menu button. 
- to development purpose keep all your plugins within the `wp-content/plugins` directory or wordpress plugin directory.
- Now reload and make your site as much as you wish.

##                        Happy Coding.......

