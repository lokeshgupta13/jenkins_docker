FROM jenkins:2.7.1
MAINTAINER Robert Watkins

USER root
RUN mkdir /var/log/jenkins
RUN mkdir /var/cache/jenkins
RUN chown -R  jenkins:jenkins /var/log/jenkins
RUN chown -R  jenkins:jenkins /var/cache/jenkins

USER jenkins
ENV JAVA_OPTS="-Xmx4096m"

# Put the log file into the log directory, which will be in the data volume
# Move the WAR out of the persisted jenkins data dir
ENV JENKINS_OPTS="--logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war"
