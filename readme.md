# Wordpress template with Docker and Traefik

This is my template to host Wordpress sites on a server.

It uses:
- Docker
- Docker Compose
- Traefik, as a reverse proxy
- Worpress (of course)
- Redis, for the cache
- MySQL
- PhPMyAdmin, for database administrations
- Let's Encrypt for SSL certificate generation

## How to deploy Wordpress

### Server and domains

Have a server with Docker installed and point your domains to your IP:

- mysite.com
- sql.mysite.com if you want to use phpmyadmin
- test1.mysite.com and test2.mysite.com if you want to learn by playing with whoami containers (see traefik/readme.md)

### Update the files

Update the files by changing placeholders:

- traefik/sample.env then rename it .env
- site/sample.env then rename it .env
- site/docker-compose.yml if you want to change the containers and networks names (needed if you have several sites)

### Launch the containers

Get into the traefik directory and launch it:

```
cd traefik
docker network create traefik
docker-compose up -d traefik
```

Then get into the site directory and launch it:

```
cd ../site
docker-compose up -d
```

Coffee time! Allow a few minutes for Let's Encrypt to generate the certificates. 

### Install and configure wordpress

Connect to mysite.com and follow Wordpress installation steps.

You also need to edit the Wordpress configuration file on the server to enable Redis:

```
sudo nano site/wordpress/data/wp_config.php
```

Add:

```
//
// redis config cache for total cache
//
define( 'W3TC_CONFIG_CACHE_ENGINE', 'redis');
define( 'W3TC_CONFIG_CACHE_REDIS_SERVERS', 'redis::6379' );
// optional redis settings
define( 'W3TC_CONFIG_CACHE_REDIS_PERSISTENT', true );
define( 'W3TC_CONFIG_CACHE_REDIS_DBID', 0 );
define( 'W3TC_CONFIG_CACHE_REDIS_PASSWORD', '' );
```

### Install the plugins

Once in Wordpress, install and configure the following plugins:

- [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/) to take advantage of Redis Cache
- [WP Mail SMTP](https://wordpress.org/plugins/wp-mail-smtp/) or similar, to make it possible to send emails from the site

I also recommend:

- [Neve](https://themeisle.com/themes/neve/) as a simple and optimized all purpose theme with a nice free version.
- [WP Forms](https://wpforms.com/) for the contact form
- [Send In Blue](https://www.sendinblue.com/plugins/wordpress/) as an SMTP provided and Newsletter handeling


## Hosting several sites on the same server

Multiple sites can be hosted on the same server thanks to Traefik reverse proxy capabilities.

Just duplicate the original ./site directory and change the container and label names.

Then launch the new site's containers. Traefik will automatically detect it.

```
cd site2
docker-compose up -d
```


## Detailed explainations

I created a tutorial there: <https://thibaut-deveraux.medium.com/a-docker-compose-file-to-install-wordpress-with-a-traefik-reverse-proxy-an-ssl-certificate-and-a-e878c2a03a17>

Each directory also contain explainations in its readme.md file.
