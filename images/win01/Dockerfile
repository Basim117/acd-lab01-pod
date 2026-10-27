FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get install -y --no-install-recommends samba \
 && rm -rf /var/lib/apt/lists/* \
 && mkdir -p /srv/share \
 && printf '[global]\n   workgroup = ACDLAB\n   netbios name = WIN01\n   server string = win01\n   map to guest = Bad User\n[share]\n   path = /srv/share\n   read only = yes\n   guest ok = yes\n' > /etc/samba/smb.conf
EXPOSE 139 445
CMD ["smbd", "--foreground", "--no-process-group", "--debug-stdout"]
