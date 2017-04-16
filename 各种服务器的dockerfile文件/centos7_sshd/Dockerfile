FROM centos

RUN  yum install net-tools openssh-server openssl  iproute  -y  
RUN  yum install vim passwd  -y

RUN ssh-keygen -q -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N '' 
RUN ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_ed25519_key  -N ''

RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config


RUN echo "redhat" | passwd --stdin root 
  

ADD run.sh /root/run.sh
RUN chmod 755 /root/run.sh

EXPOSE 22

CMD ["/root/run.sh"]
