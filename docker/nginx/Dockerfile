ARG VERSION

# Dev image
FROM nginx:${VERSION}alpine as dev

# Copy nginx config
ADD docker/nginx/default.conf /etc/nginx/conf.d/default.conf

# Prod image
FROM dev as prod

# Copy assets
ADD assets  /app/public/
