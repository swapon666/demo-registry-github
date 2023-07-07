FROM nginx:1.23-alpine AS base

COPY docker/http/nginx.conf /etc/nginx/

RUN rm -rf /etc/nginx/conf.d/*
COPY docker/http/symfony.conf /etc/nginx/conf.d/

# Expose port 80
EXPOSE 80