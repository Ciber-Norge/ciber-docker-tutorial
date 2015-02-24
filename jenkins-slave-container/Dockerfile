FROM java:openjdk-7u65-jdk

# install openssh
RUN apt-get update && apt-get install -y openssh-server

# adduser 
RUN useradd -d /home/jenkins -s /bin/bash -m jenkins
RUN echo 'jenkins:jenkins' | chpasswd

# expose ssh
EXPOSE 22

# run sshd
CMD ["/etc/init.d/ssh","start", "-D"]