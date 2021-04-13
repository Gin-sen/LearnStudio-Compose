FROM nginx:alpine

COPY ./conf/default.conf /etc/nginx/conf.d/default.conf

COPY ./conf/backend-not-found.html /var/www/html/backend-not-found.html

COPY ./conf/ssl.conf /etc/nginx/includes/
COPY ./conf/proxy.conf /etc/nginx/includes/
COPY ./conf/backend-not-found.html /etc/nginx/includes/

COPY ./certs/ /etc/ssl/certs/nginx/