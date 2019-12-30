FROM photon:latest
ARG zooversion
env JAVA_HOME /usr/lib/jvm/jre
env PATH /home/zookeeper/zookeeper/bin:/bin:/usr/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin
RUN tdnf -y install tar gzip shadow openjre8 sudo && \
	groupadd -g 10001 zookeeper && \
	useradd -d /home/zookeeper -g 10001 -u 10001 -m -s /bin/bash zookeeper && \
	echo "zookeeper ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/zookeeper
ADD --chown=zookeeper:zookeeper http://mirror.nohup.it/apache/zookeeper/zookeeper-${zooversion}/apache-zookeeper-${zooversion}-bin.tar.gz /tmp/
RUN su -c "tar -C /home/zookeeper -xzf /tmp/apache-zookeeper-${zooversion}-bin.tar.gz" zookeeper && tdnf -y clean all && rm -f /tmp/apache-zookeeper*gz && \
	su -c "find /home/zookeeper/apache-zookeeper* -maxdepth 0 -type d -exec ln -s '{}' /home/zookeeper/zookeeper ';'" zookeeper && \
        su -c "rm -f /home/zookeepeer/zookeeper/conf/zoo.cfg && cd /home/zookeeper/zookeeper/conf && ln -s /configs/zoo.cfg zoo.cfg" zookeeper && \
	su -c "mkdir /home/zookeeper/data" zookeeper && \
	tdnf -y clean all && rm -f /tmp/apache-zookeeper*gz
USER zookeeper
EXPOSE 2181
WORKDIR /home/zookeeper
CMD ["/home/zookeeper/zookeeper/bin/zkServer.sh","start-foreground"]
