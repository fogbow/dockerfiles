FROM ubuntu

WORKDIR /home/ubuntu

USER root

# Installing dependencies
RUN apt-get update
RUN apt-get install build-essential -y
RUN apt-get install git -y
RUN apt-get install curl
RUN apt-get install wget
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update
RUN apt-get install openjdk-7-jdk -y
RUN apt-get install maven -y
RUN apt-get install nfs-common -y
RUN apt-get install unzip -y
RUN apt-get install tzdata
RUN apt-get install python-pip -y
RUN apt-get install nano -y
RUN apt-get install vim -y
RUN pip install usgs

### Docker and dependencies installation ###
RUN apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common -y
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
RUN apt-get update
RUN apt-get install docker-ce -y

# Set up Fmask
WORKDIR Fmask
RUN wget http://ftp-earth.bu.edu/public/zhuzhe/Fmask_Linux_3.2v/Fmask_pkg.zip
RUN unzip Fmask_pkg.zip
RUN unzip -o MCRInstaller.zip
RUN ./install -mode silent -agreeToLicense yes

WORKDIR /home/ubuntu
RUN git clone https://github.com/fogbow/fogbow-manager.git
RUN git clone https://github.com/fogbow/blowout.git
RUN git clone https://github.com/fogbow/saps-engine.git

#MVN Install
RUN cd fogbow-manager && git checkout develop && git pull && mvn install -Dmaven.test.skip=true
RUN cd blowout && git checkout sebal-experiment-resources-fix && git pull && mvn install -Dmaven.test.skip=true
RUN cd saps-engine && git checkout frontend-integration && git pull && mvn install -Dmaven.test.skip=true


RUN sh saps-engine/scripts/infrastructure/sebal_scripts/change_timezone.sh
RUN sh saps-engine/scripts/infrastructure/sebal_scripts/install_usgs_api.sh

RUN mkdir -p /var/log/sebal-execution
RUN touch /var/log/sebal-execution/sebal-execution.log
RUN chmod 777 /var/log/sebal-execution/sebal-execution.log

RUN service docker restart

ENV LD_LIBRARY_PATH /usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/runtime/glnxa64:/usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/bin/glnxa64:/usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/sys/os/glnxa64:/usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/sys/java/jre/glnxa64/jre/lib/amd64/native_threads:/usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/sys/java/jre/glnxa64/jre/lib/amd64/server:/usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/sys/java/jre/glnxa64/jre/lib/amd64
ENV XAPPLRESDIR /usr/local/MATLAB/MATLAB_Compiler_Runtime/v81/X11/app-defaults
