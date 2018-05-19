FROM centos:centos7

RUN yum -y upgrade && \
    yum -y update && \
    yum -y install wget unzip findutils && \
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" \
    http://download.oracle.com/otn-pub/java/jdk/8u172-b11/a58eab1ec242421181065cdc37240b08/jdk-8u172-linux-x64.rpm && \
    rpm -Uvh jdk-8u172-linux-x64.rpm && \
    yum -y clean all && \
    rm -f jdk-8u172-linux-x64.rpm && rm -rf /var/cache/yum
