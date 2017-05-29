FROM geographica/postgis:latest

MAINTAINER Cayetano Benavent "cayetano.benavent@geographica.gs"


RUN apt-get -y update && \ 
  apt-get -y install alien

ENV ROOTDIR /usr/local/src

WORKDIR $ROOTDIR/

COPY rpms $ROOTDIR

RUN alien -i oracle-instantclient12.2-sqlplus-12.2.0.1.0-1.x86_64.rpm && \
  alien -i oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm && \
  alien -i oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm
    
ENV ORACLE_HOME /usr/lib/oracle/12.2/client64
ENV PATH $PATH:$ORACLE_HOME/bin
ENV OCI_LIB_DIR $ORACLE_HOME/lib
ENV OCI_INC_DIR /usr/include/oracle/12.2/client64
ENV ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:$ORACLE_HOME/lib

RUN apt-get -y update && \
  apt-get -y install git libaio1 && \
  git clone https://github.com/laurenz/oracle_fdw.git && \
  cd oracle_fdw && make && make install && \
  cd .. && rm -R oracle_fdw

RUN echo $OCI_LIB_DIR | tee /etc/ld.so.conf.d/oracle.conf && \ 
  ldconfig

EXPOSE 1571