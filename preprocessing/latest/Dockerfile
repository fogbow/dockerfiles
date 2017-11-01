FROM ubuntu:14.04

# Default
RUN apt-get -y update
RUN apt-get -y install net-tools
RUN apt-get -y install iputils-ping
RUN apt-get -y install software-properties-common

# Scheduler Dependencies
RUN add-apt-repository ppa:openjdk-r/ppa 
RUN apt-get -y update
RUN apt-get -y install openjdk-7-jdk
RUN apt-get -y install build-essential
RUN apt-get -y install git
RUN apt-get -y install maven

WORKDIR /home/ubuntu/

RUN git clone https://github.com/fogbow/fogbow-manager.git
RUN git clone -b sebal-experiment-resources-fix https://github.com/fogbow/blowout.git
RUN git clone -b frontend-integration https://github.com/fogbow/saps-engine.git
RUN cd fogbow-manager &&mvn install -DskipTests
RUN cd blowout && mvn install -DskipTests
RUN cd saps-engine && mvn install -DskipTests && mv target/saps-engine-0.0.1-SNAPSHOT.jar ..
RUN cp ./saps-engine/config/saps.conf .

