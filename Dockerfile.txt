FROM centos:8

ENV TZ=America/Sao_Paulo
WORKDIR /opt/gophish
COPY . .

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*;
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*;
RUN yum update -y && yum install passwd net-tools openssh-clients.x86_64 python3 -y
RUN yum install epel-release gcc wget git -y
RUN wget https://dl.google.com/go/go1.13.4.linux-amd64.tar.gz
RUN tar -C /usr/local -xf go1.13.4.linux-amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin
RUN source ~/.bash_profile


EXPOSE 3333 8080 8443 80
