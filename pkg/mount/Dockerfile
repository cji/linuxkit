FROM linuxkit/alpine:96ad1eb5ec262b4cd0eef574cdc0b225b502d9ee AS mirror

RUN mkdir -p /out/etc/apk && cp -r /etc/apk/* /out/etc/apk/
RUN apk add --no-cache --initdb -p /out \
    alpine-baselayout \
    busybox \
    musl \
    sfdisk \
    && true
RUN rm -rf /out/etc/apk /out/lib/apk /out/var/cache

FROM linuxkit/alpine:96ad1eb5ec262b4cd0eef574cdc0b225b502d9ee AS build

RUN apk add --no-cache go musl-dev
ENV GOPATH=/go PATH=$PATH:/go/bin

COPY *.go /go/src/mountie/
RUN go-compile.sh /go/src/mountie

FROM scratch
ENTRYPOINT []
CMD []
WORKDIR /
COPY --from=mirror /out/ /
COPY --from=build /go/bin/mountie usr/bin/mountie
CMD ["/usr/bin/mountie"]
LABEL org.mobyproject.config='{"binds": ["/dev:/dev", "/var:/var:rshared,rbind", "/:/hostroot"], "capabilities": ["CAP_SYS_ADMIN"], "rootfsPropagation": "shared", "net": "new", "ipc": "new"}'
