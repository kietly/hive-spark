FROM centos:centos7.2.1511
MAINTAINER Kiet Ly <kiet.ly@finra.org>

RUN yum -y update; yum -y install glibc-common coreutils wget ; yum clean all; wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u77-b03/jdk-8u77-linux-x64.rpm"; yum -y install jdk-8u77-linux-x64.rpm
ENV JAVA_HOME /usr/java/latest
