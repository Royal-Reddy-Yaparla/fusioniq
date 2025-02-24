FROM mysql:8.0.41
COPY scripts/* /docker-entrypoint-initdb.d/
USER root
RUN apt update && apt install -y dnsutils