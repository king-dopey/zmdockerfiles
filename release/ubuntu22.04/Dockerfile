FROM ubuntu:22.04

#Update main OS and install timezone data
RUN apt-get update -qq && \
apt-get install -y locales tzdata && \
# set the locale
locale-gen en_US.UTF-8 && \
#prepare apt 
apt-get install -y gnupg software-properties-common --no-install-recommends && \
apt-add-repository ppa:iconnor/zoneminder-1.36 -y && \
#Install ZoneMinder
DEBIAN_FRONTEND=noninteractive apt-get install -y zoneminder && \
a2enconf zoneminder && \
a2enmod rewrite cgi && \
# add local socket to mysqld config
chmod 755 /var/run/mysqld && \
echo ' \n\
[mysqld] \n\
socket=/var/run/mysqld/mysqld.sock \n' >> /etc/mysql/my.cnf && \
# clean up
apt-get remove software-properties-common -y && \
apt-get autoremove -y && \
apt-get clean && \
rm -rf /tmp/* /var/tmp/* \
/usr/share/man /usr/share/groff /usr/share/info \
/usr/share/lintian /usr/share/linda /var/cache/man && \
(( find /usr/share/doc -depth -type f ! -name copyright|xargs rm || true )) && \
(( find /usr/share/doc -empty|xargs rmdir || true ))

# Setup Volumes
VOLUME /var/cache/zoneminder/events /var/cache/zoneminder/images /var/lib/mysql /var/log/zm

# Expose http port
# Configure entrypoint

# Setup entry script
COPY utils/entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
