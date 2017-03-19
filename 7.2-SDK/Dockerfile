FROM openjdk:8-jdk

MAINTAINER Jean-Pierre FRANCONIERI <jean-pierre.franconieri@soprasteria.com>

# Install dependencies
RUN apt-get -y update && \
    apt-get install -y sudo && \
    rm -rf /var/lib/apt/lists/*

RUN useradd -m jahia && echo "jahia:jahia" | chpasswd && adduser jahia sudo

USER jahia
WORKDIR /home/jahia

# Download Digital Factory SDK
RUN wget -q \
   http://downloads.jahia.com/public/digitalexperiencemanager-7.2.0/sdk/DigitalExperienceManager-EnterpriseDistribution-7.2.0.1-SDK.zip \
   -O /tmp/DigitalExperienceManager-SDK.zip
RUN mkdir DE-7.2-SDK && \
    unzip /tmp/DigitalExperienceManager-SDK.zip && \
    rm /tmp/DigitalExperienceManager-SDK.zip

# Set env variables
ENV DX_HOME=/home/jahia/DX-EE-7.2.0.1-SDK
ENV M2_HOME=$DX_HOME/jahia/DX-EE-7.2.0.1-SDK/apache-maven-3.3.9
ENV M2=$M2_HOME/bin
ENV PATH=$M2:$PATH

# Expose Digital Factory port
EXPOSE 8080 8000

VOLUME [ "$DX_HOME/digital-factory-config", "$DX_HOME/digital-factory-data" ]

# enable JPDA remote debug
COPY scripts/setenv.sh.add /tmp
RUN cat /tmp/setenv.sh.add >> $DX_HOME/tomcat/bin/setenv.sh
# Start Tomcat
WORKDIR $DX_HOME
CMD [ "tomcat/bin/catalina.sh", "run"]
