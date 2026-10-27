FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get install -y --no-install-recommends python3 \
 && rm -rf /var/lib/apt/lists/* \
 && mkdir -p /srv/www && echo 'dev.acd.lab - test build, do not use' > /srv/www/index.html
WORKDIR /srv/www
EXPOSE 8080
CMD ["python3", "-m", "http.server", "8080"]
