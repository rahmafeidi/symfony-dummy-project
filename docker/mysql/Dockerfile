ARG VERSION

FROM mysql:5.7 as prod

# Copy sql dump to initialize tables on stratup
COPY init /docker-entrypoint-initdb.d
