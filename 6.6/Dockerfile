FROM debian:jessie

MAINTAINER Fabien Rassinier <fabien.rassinier@smile.fr>

# Install dependencies
RUN apt-get -y update && \
    apt-get install -y wget openjdk-7-jdk libreoffice && \
    rm -rf /var/lib/apt/lists/*

# Download Jahia
RUN wget -q https://www.jahia.com/downloads/jahia/jahia6.6.2/Enterprise-Jahia_xCM_v6.6.2.10_r52607.3399.jar -O /tmp/Enterprise-Jahia_xCM_v6.6.2.10_r52607.3399.jar
RUN chmod +x /tmp/Enterprise-Jahia_xCM_v6.6.2.10_r52607.3399.jar

# Install Jahia
ADD scripts /tmp
WORKDIR /tmp
RUN java -jar /tmp/Enterprise-Jahia_xCM_v6.6.2.10_r52607.3399.jar auto-install.xml
RUN ln -s /opt/Enterprise-Jahia_xCM_v6.6.2.10_r52607.3399 /opt/jahia
RUN rm -rf /tmp/*

# Expose Jahia port
EXPOSE 8080

# Start Tomcat
WORKDIR /opt/jahia/tomcat/bin
CMD ["./startup.sh"]
