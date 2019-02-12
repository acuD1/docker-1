FROM nginx:alpine

EXPOSE 80
EXPOSE 443

RUN apk update && apk add openssl
RUN mkdir -p /etc/nginx/logs /etc/nginx/ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:4096 -subj /C=FR/ST=75/L=Paris/O=rs1/OU=rs1/emailAddress=arsciand@student.42.fr -keyout /etc/nginx/ssl/selfsigned.key -out /etc/nginx/ssl/selfsigned.crt 
RUN openssl dhparam -dsaparam -out /etc/nginx/ssl/dhparam.pem 4096
CMD exec nginx -g 'daemon off;'
