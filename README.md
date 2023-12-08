# silverstripe-5-docker-example

This is an example setup for using a Docker based development environment with
a [SilverStripe 5](https://www.silverstripe.org/) CMS project, with Nginx and PHP-FPM.

This setup uses `php:8.1-fpm`, `nginx:latest`, `mysql:8.0.20` as base images, and a self-signed certificate for the ssl
port, only to be used locally.

Assuming you have an already existing project

1) Copy the `docker-compose.yml` file and copy the `docker` directory to your project

2) Generate a self-signed certificate:

    ```
    openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
    -nodes -keyout ./docker/nginx/nginx-selfsigned.key -out ./docker/nginx/nginx-selfsigned.crt -subj "/CN=example.com" \
    -addext "subjectAltName=DNS:example.com,DNS:*.example.com,IP:10.0.0.1"
    ```

3) Run `docker compose build --progress plain`

4) Run `docker compose up -d`

5) Try to access `http://localhost:8000` and/or `https://localhost:8000`

Notes:

- In the `fpm-template.conf`, use `try_files $uri /index.php?q=$query_string;` instead
  of `try_files $uri $uri/ /index.php?q=$uri&$args;` as it might break the [SilverStripe routing](https://forum.silverstripe.org/t/strange-issue-after-fresh-install/2746/10)
- The mysql server will store the data in the `./storage/db-data` - you might want to gitignore it 
- If you encounter problems with the GD extension, have a look at this [troubleshooting guide](https://underweb.dev/blog/making-php-gd-work-in-docker)
