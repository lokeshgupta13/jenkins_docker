# Hello, World

This respository contains the [Docker](https://docker.io) configuration for the IES Jenkins Build Server. Using this information, you can
swiftly setup a Jenkins server.

Note that the actual Jenkins configuration data doesn't live in this - that's internal to the running server. However, it can be exported
for backup purposes, and imported afterwards.

# Building Docker Images

Using Docker Compose:

    # Stop all the containers
    docker-compose stop
    
    # Remove all the containers
    # docker-compose rm
    # Remove just the process containers. NB: Will prompt
    
    docker-compose rm jenkinsmaster jenkinsnginx
    # build all the containers
    docker-compose build
    
    # start the continers
    docker-compose up -d
    
    # list containers
    docker-compose ps

Note that, by default, this is configured to run the ``nginx`` web server on port 80. On dev machines, this won't work - there will almost certainly be another webserver running on port 80. You can override the port by setting the ``HTTP_PORT`` environment variable when starting the Docker containers:

    export HTTP_PORT=6080
    docker-compose up -d

# Seeing the Jenkins Log
You can't ssh into the Jenkins server, but you can see the log files using ``docker exec``

    docker exec iesjenkins_jenkinsmaster_1 tail -n 1000 -f /var/log/jenkins/jenkins.log

# Backup and Restore

All the 'transient' data is stored in the ``/var/jenkins/home`` directory, which is part of the ``jenkinsdata`` container.

    # Export data to a tgz file
    docker run --rm --volumes-from iesjenkins_jenkinsdata_1 -v $(PWD):/backup ubuntu tar cvfz /backup/backup.tgz /var/jenkins_home
    
    # Import data from a tgz file.
    # This is best done with the docker containers stopped (at least the jenkinsmaster)
    docker run --rm --volumes-from iesjenkins_jenkinsdata_1 -v $(pwd):/backup ubuntu bash -c "cd /var/jenkins_home && tar xvfz /backup/backup.tgz --strip 1"
