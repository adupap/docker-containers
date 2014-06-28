############################################################
# Dockerfile to build container image of:
#   - JBoss Enterprise Application Platform (EAP) 6.2
# Based on Fedora
############################################################

# Set the base image to Fedora
FROM fedora

# File Author / Maintainer
MAINTAINER "Pedro Lopez" "plopezse@redhat.com"

# Update the repository sources list
#RUN yum update -y; yum clean all

####### BASE SOFTWARE ############
RUN yum install -y net-tools; yum clean all
RUN yum install -y java-1.7.0-openjdk; yum clean all
RUN yum install -y unzip; yum clean all



RUN yum -y install openssh-server supervisor; yum clean all
RUN mkdir /var/run/sshd
RUN echo 'root:password' |chpasswd
#RUN useradd padupa
#RUN echo 'padupa:screencast' |chpasswd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''
#ENTRYPOINT ["/usr/sbin/sshd", "-D"]

####### EAP ######################
ADD jboss-eap-6.2.0.zip /tmp/
WORKDIR /opt
RUN unzip /tmp/jboss-eap-6.2.0.zip
RUN /opt/jboss-eap-6.2/bin/add-user.sh admin admin123! --silent
ADD host-node-1.xml /opt/jboss-eap-6.2/domain/configuration/

####### OPEN PORTS ###############
EXPOSE 22
EXPOSE 8080
EXPOSE 9990
EXPOSE 9999

####### START SERVICES ###########
ADD docker_init.sh /docker_init.sh
RUN chmod +x /docker_init.sh

ADD supervisord.conf /etc/supervisord.conf
CMD ["/usr/bin/supervisord"]
#CMD ["/docker_init.sh"]
