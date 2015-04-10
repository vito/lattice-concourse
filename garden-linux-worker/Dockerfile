FROM cloudfoundry/golang-ci

# pull in dependencies for the garden-linux server
RUN \
  apt-get update && \
  apt-get -y install \
    iptables \
    quota \
    ulogd \
    zip \
  && \
  apt-get clean

RUN go get github.com/tools/godep

RUN \
  git clone \
    https://github.com/cloudfoundry-incubator/garden-linux \
    /root/go/src/github.com/cloudfoundry-incubator/garden-linux

WORKDIR /root/go/src/github.com/cloudfoundry-incubator/garden-linux
RUN godep go build -tags daemon -o garden-linux .
RUN mv garden-linux /usr/sbin/garden-linux
RUN make
RUN mv old/linux_backend/skeleton /tmp
RUN mv old/linux_backend/bin /tmp

ADD garden-linux-worker /sbin/garden-linux-worker
RUN chmod +x /sbin/garden-linux-worker

EXPOSE 7777

CMD /sbin/garden-linux-worker
