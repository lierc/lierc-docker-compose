FROM debian:jessie
EXPOSE 80 443
WORKDIR /opt/h2o
RUN apt-get update
RUN apt-get -y install build-essential cmake git libssl-dev
RUN git clone https://github.com/h2o/h2o.git /tmp/h2o
RUN cd /tmp/h2o && git submodule init && git submodule update && cmake -DWITH_BUNDLED_SSL=on . && make install
COPY h2o.conf /etc/h2o/h2o.conf
CMD h2o -c /etc/h2o/h2o.conf
