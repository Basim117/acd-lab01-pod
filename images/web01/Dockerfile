FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get install -y --no-install-recommends apache2 openssh-server \
 && rm -rf /var/lib/apt/lists/* \
 && mkdir -p /run/sshd \
 && echo '<h1>www.acd.lab</h1><p>ACD lab web server (web01)</p>' > /var/www/html/index.html
EXPOSE 22 80
CMD ["bash", "-c", "/usr/sbin/sshd && exec apachectl -D FOREGROUND"]
