FROM docker.io/telegraf:latest

RUN apt-get -q -y update \
  && DEBIAN_FRONTEND=noninteractive apt-get -q -y upgrade \
  && DEBIAN_FRONTEND=noninteractive apt-get -q -y install \
  python3 \
  python3-requests \
  incron \
  && DEBIAN_FRONTEND=noninteractive apt-get -q clean \
  && rm -rf /var/lib/apt/lists/*

ADD ./send_http_req.py /telegraf/send_http_req.py
ADD ./send_http_req_sentry.py /telegraf/send_http_req_sentry.py

# Configure trigger for restarting app on log rotation
RUN rm /etc/incron.allow
ADD incron.conf /telegraf/telegraf_incron.conf
RUN incrontab /telegraf/telegraf_incron.conf

COPY entrypoint-extended.sh /
RUN chmod +x /entrypoint-extended.sh

ENTRYPOINT ["/entrypoint-extended.sh"]

CMD ["telegraf"]
