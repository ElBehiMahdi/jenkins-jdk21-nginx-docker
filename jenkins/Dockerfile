FROM jenkins/jenkins:lts-jdk17

# add ability to run docker from within jenkins (docker in docker)
USER root

# Install dependencies
RUN apt-get update && apt-get install -y \
    lsb-release \
    sudo \
    gosu

# Install Docker CLI
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER jenkins

# Environment variables
ENV JENKINS_UID=1000
ENV JENKINS_GID=1000

# Copy entrypoint script
COPY docker-entrypoint.sh /docker-entrypoint.sh


USER root

# Use correct tini path
ENTRYPOINT ["/usr/bin/tini", "--", "/docker-entrypoint.sh"]
