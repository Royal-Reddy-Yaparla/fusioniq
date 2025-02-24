FROM mysql:8.0.41

# Copy initialization scripts
COPY scripts/* /docker-entrypoint-initdb.d/

# Switch to root user to install packages
USER root

# Update package lists and install dnsutils
RUN apt update && apt install -y dnsutils

# Switch back to MySQL user for security
USER mysql
