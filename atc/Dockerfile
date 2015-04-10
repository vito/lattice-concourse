FROM cloudfoundry/golang-ci

RUN \
  git clone \
    --branch develop \
    https://github.com/concourse/concourse \
    /root/concourse

WORKDIR /root/concourse

RUN git submodule update --init --recursive

ENV GOPATH /root/concourse

RUN go build -o /usr/sbin/atc github.com/concourse/atc/cmd/atc && \
    cp -a src/github.com/concourse/atc/web/templates /tmp/templates && \
    cp -a src/github.com/concourse/atc/web/public /tmp/public && \
    rm -rf /root/concourse

WORKDIR /

EXPOSE 8080

ADD atc /sbin/atc
RUN chmod +x /sbin/atc
CMD /sbin/atc
