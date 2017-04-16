# Centos based container with Java and Tomcat

#
FROM centos

#
MAINTAINER jacky zheng,cnhidba@126.com

#  
ENV JAVA_HOME /usr/java/jdk1.7.0_79
ENV CATALINA_HOME /usr/local/tomcat 
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_HOME/scripts

# jdk
COPY jdk-7u79-linux-x64.rpm /data/
RUN cd /data && \
 rpm -ivh jdk-7u79-linux-x64.rpm


# tomcat

COPY apache-tomcat-7.0.34.tar.gz /data/

RUN cd /data && \
 tar -xvf apache-tomcat-7.0.34.tar.gz && \
 rm apache-tomcat*.tar.gz && \
 mv apache-tomcat* ${CATALINA_HOME}

RUN chmod +x ${CATALINA_HOME}/bin/*sh

# 
ADD create_admin_user.sh $CATALINA_HOME/scripts/create_admin_user.sh
ADD tomcat.sh $CATALINA_HOME/scripts/tomcat.sh
RUN chmod +x $CATALINA_HOME/scripts/*.sh

# 
RUN groupadd -r tomcat && \
 useradd -g tomcat -d ${CATALINA_HOME} -s /sbin/nologin  -c "Tomcat user" tomcat && \
 chown -R tomcat:tomcat ${CATALINA_HOME}

#tomcat apr 
COPY apr-1.5.2.tar.gz /data/
RUN cd /data && \
tar xvzf apr-1.5.2.tar.gz && cd apr-1.5.2 && \ 
./configure --with-apr=/usr/bin/apr-1-config && make && make install

RUN cd /usr/local/tomcat/bin/ && \
tar zxf tomcat-native.tar.gz && cd tomcat-native-1.1.24-src/jni/native/ && ./configure --with-apr=/usr/local/apr &&  \
make && make install

RUN yum install tomcat-native.x86_64 -y

ADD catalina.sh $CATALINA_HOME/bin/
ADD server.xml $CATALINA_HOME/conf/

#
WORKDIR /data

#
EXPOSE 8080 8009

RUN chmod 777 $CATALINA_HOME/bin/*.sh

CMD ["tomcat.sh"]
