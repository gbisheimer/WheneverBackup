FROM phusion/passenger-ruby22
MAINTAINER ByS Control "info@bys-control.com.ar"

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Instalo gemas para manejo de backups
RUN gem install whenever backup && \
apt-get udpate && \
apt-get install -y --no-install-recommends supervisor && \
rm -rf /var/lib/apt/lists/* && \

CMD ["/usr/bin/supervisord"]