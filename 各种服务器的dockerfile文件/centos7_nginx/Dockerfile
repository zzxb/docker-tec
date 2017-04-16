#基础linux版本
FROM centos
#作者信息
MAINTAINER jacky zheng,cnhidba@126.com
#进入容器后当前目录设置
WORKDIR /usr/local/src/

#复制宿主容器外的文件或目录到容器内
COPY pcre-8.37.tar.gz    .
COPY nginx-1.8.0.tar.gz  .

#添加用户并安装编译插件
#RUN useradd -s /sbin/nologin www  && yum install gcc gcc-c++ openssl openssl-devel -y

#安装编译插件
RUN yum install gcc gcc-c++ openssl openssl-devel -y

#解压缩并编译pcre
RUN tar zxf pcre-8.37.tar.gz   &&  cd pcre-8.37 && ./configure --prefix=/usr/local/pcre  && make && make install

#解压缩并编译nginx
RUN tar zxf nginx-1.8.0.tar.gz && cd nginx-1.8.0 && ./configure --prefix=/usr/local/nginx  \
--user=www  \
--group=www \
--with-http_ssl_module  \
--with-http_stub_status_module \ 
--with-file-aio  \
--with-http_dav_module \
--with-pcre=/usr/local/src/pcre-8.37 && make && make install && chown -R www. /usr/local/nginx

#复制宿主容器外配置文件到容器内
COPY nginx.conf /usr/local/nginx/conf/nginx.conf
ADD vhosts /usr/local/nginx/conf/vhosts

#创建数据卷，将所有应用程序放到数据卷中
#VOLUME ["/data"]

#复制运行文件到容器内
ADD run.sh /root/run.sh

#修改运行权限
RUN chmod 755 /root/run.sh

#执行
CMD ["/root/run.sh"]

#开放的端口
EXPOSE 80 443
