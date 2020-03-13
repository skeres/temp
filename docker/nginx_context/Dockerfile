# nginx image on ubunu server
# BEFORE running tthis file
# a user "deploy" must have been create ( set password to deploy too... )  : sudo adduser deploy; usermod -aG udo deploy
# generate sshkey without passphrase and default files : sudo su - deploy ; ssh-keygen -t rsa
# add user deploy to a docker group so deploy can use docker : sudo groupadd docker ; sudo usermod -aG docker deploy
# disconnect from terminal and reconnect as deploy user
# verify everthing's ok, this should run : docker run hello-world
FROM nginx:stable
# public key of the user named deploy. Must be define with existing public and private key
ARG arg1
# install ssh server and tools
RUN apt-get update && apt-get install -y openssh-server
RUN apt-get install -y vim
# create user named deploy on server, with no password, and add it to sudoers
RUN adduser --disabled-password --gecos "" deploy;
# create file authorized_keys
RUN mkdir -p /home/deploy/.ssh && touch /home/deploy/.ssh/authorized_keys;
# set deploy user's public key 
RUN echo "$arg1" > /home/deploy/.ssh/authorized_keys;
# add prerequisite for ansible
RUN apt-get install -y python-minimal;
# install sudo
RUN apt-get install -y sudo; usermod -aG sudo deploy;

RUN touch /etc/sudoers.d/addSudo; chmod 440 /etc/sudoers.d/addSudo; echo 'deploy  ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers.d/addSudo; 

#start ssh service and launch nginx
CMD service ssh start && nginx -g 'daemon off;'
