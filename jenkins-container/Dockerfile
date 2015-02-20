FROM java:openjdk-7u65-jdk

RUN apt-get update && apt-get install -y wget git curl zip net-tools && rm -rf /var/lib/apt/lists/*
RUN wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
RUN echo deb http://pkg.jenkins-ci.org/debian binary/ >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y jenkins && rm -rf /var/lib/apt/lists/*
#ENV JENKINS_HOME /etc/default/jenkins
# Jenkins home directoy is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME /var/lib/jenkins


# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000

ENTRYPOINT ["/etc/init.d/jenkins"]
CMD ["start"]
CMD read

