FROM ubuntu:14.04
MAINTAINER Robert Schönthal <robert.schoenthal@gmail.com>

RUN apt-get update && apt-get -qy upgrade && apt-get install -qy python-software-properties software-properties-common unzip
RUN add-apt-repository -y ppa:nginx/stable && apt-get update

#Install Nginx
RUN apt-get install -y nginx
RUN chown -R www-data:www-data /var/lib/nginx

# Install Serf
ADD https://dl.bintray.com/mitchellh/serf/0.5.0_linux_amd64.zip serf.zip
RUN unzip serf.zip
RUN mv serf /usr/bin/
ADD serf-member-join.sh /serf-member-join.sh
ADD serf-member-leave.sh /serf-member-leave.sh
ADD serf-join.sh /serf-join.sh
RUN chmod 755 /*.sh

# Install Supervisor
RUN apt-get install -qy supervisor
ADD /supervisord-nginx.conf /etc/supervisor/supervisord.conf

# Configure Nginx
ADD /nginx.conf /etc/nginx/nginx.conf

RUN mkdir -p srv/www

# Add Hosts
ADD /hosts/default.conf /etc/nginx/sites-enabled/default

VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/srv/www"]

EXPOSE 81

CMD ["/usr/bin/supervisord"]
