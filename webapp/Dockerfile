FROM phusion/passenger-ruby20
# Set correct environment variables.
ENV HOME /root
# Use baseimage-docker's init process.
CMD ["/sbin/my_init"]
#   Build system and git.
RUN /build/utilities.sh
#   Ruby support.
RUN /build/ruby2.0.sh
#   Common development headers necessary for many Ruby gems,
#   e.g. libxml for Nokogiri.
#RUN /build/devheaders.sh
#   Node.js and Meteor support.
RUN /build/nodejs.sh

RUN apt-get update; \
    apt-get install -y libmysqlclient-dev

# the app
RUN mkdir /home/app/webapp
ADD src/webapp /home/app/webapp
RUN chown -R app /home/app/webapp

WORKDIR /home/app/webapp
RUN sudo -u app -H bundle install --path vendor/bundle

# nginx
ADD files/webapp.conf /etc/nginx/sites-enabled/webapp.conf
RUN rm -f /etc/service/nginx/down

#EXPOSE 80

# ssh
#RUN /usr/sbin/enable_insecure_key
ADD files/id_rsa.pub /tmp/id_rsa.pub
RUN cat /tmp/id_rsa.pub >> /root/.ssh/authorized_keys && rm -f /tmp/id_rsa.pub

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

