FROM debian:jessie

MAINTAINER Fabien Rassinier <fabien.rassinier@smile.fr>

# Install dependencies
RUN apt-get -y update && \
    apt-get install -y wget unzip openjdk-7-jdk git subversion libreoffice && \
    rm -rf /var/lib/apt/lists/*

# Download Digital Factory
RUN wget -q https://www.jahia.com/downloads/jahia/digitalexperiencemanager7.1.1/DigitalExperienceManager-EnterpriseDistribution-7.1.1.0-r53717.3663.jar -O /tmp/DigitalExperienceManager.jar
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
ADD scripts /tmp
WORKDIR /tmp
RUN java -jar /tmp/DigitalExperienceManager.jar auto-install.xml
RUN ln -s /opt/DigitalExperienceManager-EnterpriseDistribution-7.1.1.0 /opt/DigitalExperienceManager
RUN rm -rf /tmp/*

# Expose Digital Factory port
EXPOSE 8080

# Start Tomcat
WORKDIR /opt/DigitalExperienceManager/tomcat/bin
CMD ["./startup.sh"]
