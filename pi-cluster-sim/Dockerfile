# Use Ubuntu 22.04 base for x64 (more recent and better support)
FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install GlusterFS from Ubuntu repositories
RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:gluster/glusterfs-10 && \
    apt-get update && \
    apt-get install -y glusterfs-server glusterfs-client ssh net-tools iproute2 sudo curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy the setup script into the image
COPY setup.sh /usr/local/bin/setup.sh
RUN chmod +x /usr/local/bin/setup.sh

# Create GlusterFS brick directory
RUN mkdir -p /data/brick

# Create necessary directories for GlusterFS
RUN mkdir -p /var/lib/glusterd /var/log/glusterfs

# Expose GlusterFS ports
EXPOSE 24007 24008 49152-49251

# Run the setup script during container startup
CMD ["/bin/bash", "-c", "/usr/local/bin/setup.sh && /usr/sbin/glusterd -N"]