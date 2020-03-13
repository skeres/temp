# nodejs on ubunu server
# BEFORE running this file
# a user "deploy" must have been create ( set password to deploy too... )  : sudo adduser deploy; usermod -aG udo deploy
# generate sshkey without passphrase and default files : sudo su - deploy ; ssh-keygen -t rsa
# add user deploy to a docker group so deploy can use docker : sudo groupadd docker ; sudo usermod -aG docker deploy
# disconnect from terminal and reconnect as deploy user
FROM ubuntu:18.04
# public key of the user named deploy. Must be define with existing public and private key
ARG arg1
# install ssh server and tools
RUN apt-get update && apt-get install -y openssh-server
RUN apt-get install -y vim
# create user named deploy on server, with no password, and add it to sudoers
RUN adduser --disabled-password --gecos "" deploy;
# create file authorized_keys
RUN mkdir -p /home/deploy/.ssh && touch /home/deploy/.ssh/authorized_keys;
RUN chown -R deploy:deploy /home/deploy/ && chmod 744 -R /home/deploy/;
# set deploy user's public key 
RUN echo "$arg1" > /home/deploy/.ssh/authorized_keys;
#set password not mandatory for ssh 
RUN sed -i 's/#PasswordAuthentication\ yes/PasswordAuthentication\ no/' /etc/ssh/sshd_config;
RUN sed -i 's/#\ChallengeResponseAuthentication\ yes/ChallengeResponseAuthentication\ no/' /etc/ssh/sshd_config;
RUN sed -i 's/UsePAM\ yes/UsePAM\ no/' /etc/ssh/sshd_config;
# add prerequisite for ansible
RUN apt-get install -y python;
# install sudo
RUN apt-get install -y sudo; usermod -aG sudo deploy;

RUN touch /etc/sudoers.d/addSudo; chmod 440 /etc/sudoers.d/addSudo; echo 'deploy  ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers.d/addSudo; 

# needed directory for sshd. At install, sometimes, this directory is not created
RUN mkdir -p /run/sshd

#start ssh service and launch nginx
#CMD service ssh start && ["/bin/bash"] 
CMD ["/usr/sbin/sshd","-D"]
