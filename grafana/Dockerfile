FROM ubuntu:trusty

ENV BUILD_VERSION 4.1.2
RUN apt-get update && apt-get -y install libfontconfig wget adduser openssl ca-certificates curl supervisor

RUN wget -O grafana_latest_amd64.deb https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.3.0-beta1_amd64.deb
RUN dpkg -i grafana_latest_amd64.deb

EXPOSE 3000

VOLUME ["/usr/share/grafana/data"]

WORKDIR /usr/share/grafana

ADD dashboards/import_format/ /etc/grafana/dashboards/
ADD load.sh /etc/grafana/load.sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
