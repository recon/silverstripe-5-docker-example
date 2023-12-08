FROM nginx:latest

ENV NGINX_ROOT /usr/src/app/public
ENV NGINX_FPM_HOST localhost
ENV NGINX_FPM_PORT 9000

RUN rm -f /etc/nginx/conf.d/default.conf
RUN rm -f /etc/nginx/nginx.conf

COPY ./docker/nginx/nginx-selfsigned.crt /etc/nginx/ssl/nginx-selfsigned.crt
COPY ./docker/nginx/nginx-selfsigned.key /etc/nginx/ssl/nginx-selfsigned.key


COPY ./docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./docker/nginx/fpm-template.conf /etc/nginx/fpm.tmpl
COPY ./docker/nginx/entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod +x /usr/local/bin/entrypoint.sh
RUN mkdir /usr/src/app && chown 1000:1000 /usr/src/app && chmod 0755 /usr/src/app
RUN mkdir /usr/src/app/public && chown 1000:1000 /usr/src/app/public && chmod 0755 /usr/src/app/public

EXPOSE 80

ENTRYPOINT ["entrypoint.sh"]
