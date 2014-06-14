#########use ssh_command file for the list of commands
docker build --rm -t padupa/ssh .
docker run -d -p 22 padupa/ssh
ssh -p 49156 root@localhost

#############below files needs to be in the same directory of Dockerfile
brms-coolstore-demo-master.zip
jboss-bpms-6.0.1.GA-redhat-4-deployable-eap6.x.zip
jboss-eap-6.1.1.zip
