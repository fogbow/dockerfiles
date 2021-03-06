FROM ubuntu:14.04

# Default
RUN apt-get -y update
RUN apt-get -y install net-tools
RUN apt-get -y install iputils-ping
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa 
RUN apt-get -y update

# Worker Depedencies
RUN apt-get -y install openjdk-7-jdk
RUN apt-get -y install build-essential
RUN apt-get -y install ant
RUN apt-get -y install swig
RUN apt-get -y install libnetcdf-dev
RUN apt-get -y install netcdf-bin
RUN apt-get -y install libgdal1-dev 
RUN apt-get -y install libproj-dev
RUN apt-get -y install unzip
RUN apt-get -y install nfs-common
RUN apt-get -y install git
RUN apt-get -y install maven

WORKDIR /home/ubuntu/

# Installing R
RUN echo 'deb http://nbcgib.uesc.br/mirrors/cran/bin/linux/ubuntu trusty/' >> /etc/apt/sources.list
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 51716619E084DAB9
RUN apt-get -y update
RUN apt-get install -y --force-yes r-base 
RUN apt-get install -y r-base-dev

# Exporting env variables
RUN export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
RUN export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /root/.bashrc
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /root/.profile
RUN echo 'export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7' >> /root/.bashrc
RUN echo 'export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7' >> /root/.profile
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /home/ubuntu/.bashrc
RUN echo 'export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/' >> /home/ubuntu/.profile
RUN echo 'export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7' >> /home/ubuntu/.bashrc
RUN echo 'export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7' >> /home/ubuntu/.profile
RUN /bin/bash -c "source /home/ubuntu/.profile"

# Installing wget
RUN apt-get install -y wget
RUN rm -rf /var/lib/apt/lists/*

# Installing gdal
RUN wget http://download.osgeo.org/gdal/1.10.1/gdal-1.10.1.tar.gz
RUN tar -xvzf gdal-1.10.1.tar.gz
RUN gdal-1.10.1/configure
RUN cd gdal-1.10.1 && ./configure && make -e && make install
RUN export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/ && export LD_LIBRARY_PATH=/path/para/diretório/include/dentro/do/jdk7 && cd gdal-1.10.1/swig/java && make -e

# Fmask
RUN mkdir Fmask
RUN cd Fmask/ && wget http://ftp-earth.bu.edu/public/zhuzhe/Fmask_Linux_3.2v/Fmask_pkg.zip
RUN cd Fmask/ && unzip Fmask_pkg.zip
# Unzip -o is important because there is a same file in the folder. Is necessary overrite.
RUN cd Fmask/ && unzip -o MCRInstaller.zip
RUN cd Fmask/ && ./install -mode silent -agreeToLicense yes
RUN rm Fmask/Fmask_pkg.zip
RUN rm Fmask/MCRInstaller.zip

# Install R packeges
RUN R -e 'install.packages("raster", repos="https://vps.fmvz.usp.br/CRAN/", dependencies=TRUE, INSTALL_opts=c("--no-lock"))'
RUN R -e 'install.packages("rgeos", repos="https://vps.fmvz.usp.br/CRAN/", dependencies=TRUE, INSTALL_opts=c("--no-lock"))'
RUN R -e 'install.packages("rgdal", repos="https://vps.fmvz.usp.br/CRAN/", dependencies=TRUE, INSTALL_opts=c("--no-lock"))'
RUN R -e 'install.packages("maptools", repos="https://vps.fmvz.usp.br/CRAN/", dependencies=TRUE, INSTALL_opts=c("--no-lock"))'
RUN R -e 'install.packages("ncdf4", repos="https://vps.fmvz.usp.br/CRAN/", dependencies = TRUE, INSTALL_opts = c("--no-lock"))'
RUN R -e 'install.packages("sp", repos="https://vps.fmvz.usp.br/CRAN/", dependencies = TRUE, INSTALL_opts = c("--no-lock"))'
RUN R -e 'install.packages("R.utils", repos="https://vps.fmvz.usp.br/CRAN/", dependencies = TRUE, INSTALL_opts = c("--no-lock"))'
RUN R -e 'install.packages("snow", repos="https://vps.fmvz.usp.br/CRAN/")'

RUN apt-get -y update

RUN apt-get -y install sysstat

WORKDIR /home/ubuntu

COPY ./SEBAL/ .
