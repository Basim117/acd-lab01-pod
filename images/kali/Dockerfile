ARG BASE=kalilinux/kali-rolling
FROM ${BASE}
ENV DEBIAN_FRONTEND=noninteractive USER=root
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      nmap fping arp-scan traceroute iputils-ping iproute2 curl xsltproc \
      python3 bsdextrautils less nano sudo ca-certificates \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /root
CMD ["sleep", "infinity"]
