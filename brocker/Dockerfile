FROM photon:latest
ARG kafkaversion
env JAVA_HOME /usr/lib/jvm/jre
env PATH /home/kafka/kafka/bin:/bin:/usr/bin:/usr/local/bin:/sbin:/usr/sbin:/usr/local/sbin

RUN tdnf -y erase toybox && tdnf -y install gawk procps-ng iputils sed findutils tar gzip shadow openjre8 sudo && \
	groupadd -g 10001 kafka && \
	useradd -d /home/kafka -g 10001 -u 10001 -m -s /bin/bash kafka && \
	echo "kafka ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/kafka
ADD --chown=kafka:kafka http://mirror.nohup.it/apache/kafka/2.4.0/kafka_2.12-${kafkaversion}.tgz /tmp/
RUN su -c "tar -C /home/kafka -xzf /tmp/kafka_2.12-${kafkaversion}.tgz" kafka && \
	su -c "rm -f /home/kafka/kafka_2.12-${kafkaversion}/config/server.properties && ln -sf /configs/server.properties /home/kafka/kafka_2.12-${kafkaversion}/config/server.properties" kafka && \
	su -c "ln -s /home/kafka/kafka_2.12-${kafkaversion} /home/kafka/kafka" kafka && \
	tdnf -y clean all && rm -f /tmp/kafka*gz
USER kafka
WORKDIR /home/kafka
CMD ["/home/kafka/kafka/bin/kafka-server-start.sh", "/home/kafka/kafka/config/server.properties"]
