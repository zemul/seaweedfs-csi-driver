FROM golang:1.24-alpine as builder
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN apk add git g++

#RUN mkdir -p /go/src/github.com/seaweedfs/
#RUN git clone https://github.com/seaweedfs/seaweedfs /go/src/github.com/seaweedfs/seaweedfs
#RUN cd /go/src/github.com/seaweedfs/seaweedfs/weed && go install

RUN mkdir -p /go/src/github.com/zemul/
RUN git clone https://github.com/zemul/seaweedfs-csi-driver /go/src/github.com/zemul/seaweedfs-csi-driver
RUN cd /go/src/github.com/zemul/seaweedfs-csi-driver && go build -ldflags="-s -w" -o /seaweedfs-csi-driver ./cmd/seaweedfs-csi-driver/main.go

FROM alpine AS final
RUN apk add fuse
LABEL author="Chris Lu"
COPY --from=builder /seaweedfs-csi-driver /

RUN chmod +x /seaweedfs-csi-driver
ENTRYPOINT ["/seaweedfs-csi-driver"]
