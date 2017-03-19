FROM openjdk:8-jdk

MAINTAINER Fabien Rassinier <fabien.rassinier@smile.fr>

# Install dependencies
RUN apt-get -y update && \
    apt-get install -y wget unzip libreoffice && \
    rm -rf /var/lib/apt/lists/*

#VOLUME [ "/opt/Jahia-DExMgr-7.2.0.1/digital-factory-config", "/opt/Jahia-DExMgr-7.2.0.1/digital-factory-data" ]

# Download Digital Factory
RUN wget -q http://downloads.jahia.com/downloads/jahia/digitalexperiencemanager7.2.0/DigitalExperienceManager-EnterpriseDistribution-7.2.0.1-r55404.4001.jar -O /tmp/DigitalExperienceManager.jar
RUN chmod +x /tmp/DigitalExperienceManager.jar

# Download Maven 3
RUN wget -q http://wwwftp.ciril.fr/pub/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.zip -O /tmp/apache-maven-3.3.9-bin.zip
RUN unzip /tmp/apache-maven-3.3.9-bin.zip -d /usr/share
RUN ln -s /usr/share/apache-maven-3.3.9 /usr/share/maven

# Set env variables
ENV M2_HOME=/usr/share/maven
ENV M2=/usr/share/maven/bin
ENV PATH=/usr/share/maven/bin:$PATH

# Install Digital Factory
COPY scripts/auto-install.xml /tmp
WORKDIR /tmp
RUN java -jar /tmp/DigitalExperienceManager.jar auto-install.xml
RUN ln -s /opt/Jahia-DExMgr-7.2.0.1 /opt/DigitalExperienceManager
RUN rm -rf /tmp/*

# Expose Digital Factory port
EXPOSE 8080

# Start Tomcat
WORKDIR /opt/DigitalExperienceManager/tomcat/bin
CMD ["./catalina.sh", "run"]
