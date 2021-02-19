FROM jenkins/jenkins:latest

ARG HOST_UID=1000
ARG HOST_GID=994

ENV JENKINS_SLAVE_AGENT_PORT 50000

COPY resources/plugins/plugins.txt /usr/share/jenkins/ref/
COPY resources/selenium-server-standalone-3.141.59.jar /home/
RUN /usr/local/bin/install-plugins.sh $(cat /usr/share/jenkins/ref/plugins.txt)

COPY resources/users/*.groovy /usr/share/jenkins/ref/init.groovy.d/

#RUN sudo chown -R 1000:1000 /opt/jenkins

USER root
RUN apt-get -y update && \
 apt-get -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common && \
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
 add-apt-repository \
 "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
 $(lsb_release -cs) \
 stable" && \
 apt-get update && \
 apt-get -y install docker-ce docker-ce-cli containerd.io
RUN curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
 chmod +x /usr/local/bin/docker-compose && \
 ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
RUN usermod -u $HOST_UID jenkins
RUN groupmod -g $HOST_GID docker
RUN usermod -aG docker jenkins
USER jenkins
