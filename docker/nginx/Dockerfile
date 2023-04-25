ARG VERSION

# Dev image
FROM nginx:${VERSION}alpine as dev

# Copy nginx config
COPY ./docker/nginx/default.conf /etc/nginx/conf.d/default.conf

# Prod image
FROM nginx:${VERSION}alpine as prod

# Copy assets
COPY ./assets /app/public
