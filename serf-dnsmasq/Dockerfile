# Creates a base ubuntu image with serf ,ssh and dnsmasq 
FROM ubuntu:15.04

MAINTAINER Young <361242902@qq.com>

RUN apt-get update && \
apt-get install -y unzip curl dnsmasq openssh-server vim

ADD files/* /usr/local/

#configure ssh free key access
RUN mkdir /var/run/sshd && \
ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
mv /usr/local/ssh_config ~/.ssh/config && \
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# dnsmasq configuration
ADD dnsmasq/* /etc/

# install serf
RUN curl -Lso serf.zip https://dl.bintray.com/mitchellh/serf/0.5.0_linux_amd64.zip && \
unzip serf.zip -d /bin && \
rm serf.zip

# configure serf
ENV SERF_CONFIG_DIR /etc/serf
ADD serf/* $SERF_CONFIG_DIR/
ADD handlers $SERF_CONFIG_DIR/handlers
RUN chmod +x  $SERF_CONFIG_DIR/event-router.sh $SERF_CONFIG_DIR/start-serf-agent.sh
