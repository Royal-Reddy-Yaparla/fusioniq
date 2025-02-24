FROM mysql:8.0.41
COPY scripts/* /docker-entrypoint-initdb.d/
USER root
RUN yum update && yum install -y dnsutils