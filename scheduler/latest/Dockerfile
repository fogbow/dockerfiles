FROM ubuntu:14.04

# Default
RUN apt-get -y update
RUN apt-get -y install net-tools
RUN apt-get -y install iputils-ping
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa 
RUN apt-get -y update

# Scheduler Dependencies
RUN apt-get -y install openjdk-7-jdk
RUN apt-get -y install build-essential
RUN apt-get -y install unzip
RUN apt-get -y install git
RUN apt-get -y install maven
RUN apt-get -y install ntp

WORKDIR /home/ubuntu/

# Scheduler Configurations
# Timezone
RUN echo "America/Recife" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# NTP Client
RUN sed -i "/server 0.ubuntu.pool.ntp.org/d" /etc/ntp.conf
RUN sed -i "/server 1.ubuntu.pool.ntp.org/d" /etc/ntp.conf
RUN sed -i "/server 2.ubuntu.pool.ntp.org/d" /etc/ntp.conf
RUN sed -i "/server 3.ubuntu.pool.ntp.org/d" /etc/ntp.conf
RUN sed -i "/server ntp.ubuntu.com/d" /etc/ntp.conf
RUN echo "server ntp.lsd.ufcg.edu.br" >> /etc/ntp.conf
RUN service ntp restart

# Exporting env variables
RUN export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /root/.bashrc
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /root/.profile
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /home/ubuntu/.bashrc
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /home/ubuntu/.profile

RUN git clone https://github.com/fogbow/fogbow-manager.git
RUN git clone https://github.com/fogbow/blowout.git
RUN git clone https://github.com/fogbow/saps-engine.git

RUN cd fogbow-manager && git checkout develop
RUN cd blowout && git checkout sebal-experiment-resources-fix
RUN cd saps-engine && git checkout frontend-integration

RUN cd fogbow-manager && mvn -e install -Dmaven.test.skip=true
RUN cd blowout && mvn -e install -Dmaven.test.skip=true
RUN cd saps-engine && mvn -e install -Dmaven.test.skip=true
