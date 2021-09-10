FROM nginx:alpine

# ADD Custom Config
ADD ./compose/nginx/nginx.conf /etc/nginx/nginx.conf
ADD ./compose/nginx/default.conf /etc/nginx/conf.d/default.conf

# Make Directory - Workspace
RUN mkdir -p /var/www/html

