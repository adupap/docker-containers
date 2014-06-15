FROM fedora
MAINTAINER scollier <scollier@redhat.com>

RUN yum -y update; yum clean all
RUN yum -y install openssh-server; yum clean all
RUN mkdir /var/run/sshd
RUN echo 'root:screencast' |chpasswd
#RUN useradd padupa
#RUN echo 'padupa:screencast' |chpasswd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N '' 
ENTRYPOINT ["/usr/sbin/sshd", "-D"]


####### BASE SOFTWARE ############
RUN yum install -y net-tools
RUN yum install -y java-1.7.0-openjdk
RUN yum install -y unzip
RUN yum install -y git 
RUN yum install -y maven


WORKDIR /opt

####### EAP ######################
ADD jboss-eap-6.1.1.zip /tmp/
RUN unzip /tmp/jboss-eap-6.1.1.zip

####### BPMS######################
ADD jboss-bpms-6.0.1.GA-redhat-4-deployable-eap6.x.zip /tmp/
RUN unzip -o /tmp/jboss-bpms-6.0.1.GA-redhat-4-deployable-eap6.x.zip

####### COOLSTORE######################
ADD brms-coolstore-demo-master.zip /tmp/
RUN unzip /tmp/brms-coolstore-demo-master.zip

###############ENV#####################
ENV JBOSS_HOME /opt/jboss-eap-6.1
ENV SERVER_DIR $JBOSS_HOME/standalone/deployments
ENV SERVER_CONF $JBOSS_HOME/standalone/configuration
ENV SERVER_BIN $JBOSS_HOME/bin
ENV SUPPORT_LIBS /opt/brms-coolstore-demo-master/support/libs
ENV SUPPORT_DIR /opt/brms-coolstore-demo-master/support
ENV WEB_INF_LIB /opt/brms-coolstore-demo-master/projects/brms-coolstore-demo/src/main/webapp/WEB-INF/lib/


RUN mvn install:install-file -Dfile=$SUPPORT_LIBS/cdiutils-1.0.0.jar -DgroupId=org.vaadin.virkki -DartifactId=cdiutils -Dversion=1.0.0 -Dpackaging=jar
RUN mvn install:install-file -Dfile=$SUPPORT_LIBS/coolstore-2.0.0.jar -DgroupId=com.redhat -DartifactId=coolstore -Dversion=2.0.0 -Dpackaging=jar
RUN mkdir -p $WEB_INF_LIB
RUN cp $SUPPORT_LIBS/cdiutils-1.0.0.jar $WEB_INF_LIB
RUN cp -r $SUPPORT_DIR/brms-demo-niogit $SERVER_BIN/.niogit


RUN /opt/jboss-eap-6.1/bin/add-user.sh admin admin123! --silent
RUN /opt/jboss-eap-6.1/bin/add-user.sh -a --silent --role admin,developer,analyst,user,manager --user jbossadmin --password admin123!
RUN /opt/jboss-eap-6.1/bin/add-user.sh -a --silent --role user,analyst,appraiser,broker,manager --user jbossuser --password user123!

RUN cp $SUPPORT_DIR/standalone.xml /opt/jboss-eap-6.1/standalone/configuration/

WORKDIR /opt/brms-coolstore-demo-master/projects/brms-coolstore-demo
RUN mvn clean install
RUN cp /opt/brms-coolstore-demo-master/projects/brms-coolstore-demo/target/brms-coolstore-demo.war $SERVER_DIR

####### OPEN PORTS ###############
#EXPOSE 22
EXPOSE 8080
EXPOSE 9990
EXPOSE 9999

RUN rm -rf /tmp/*.zip
RUN rm -rf /tmp/brms*
####### START SERVICES ###########
#ADD docker_init.sh /docker_init.sh
#RUN chmod +x /docker_init.sh

#CMD ["/docker_init.sh"]

