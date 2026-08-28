ARG GO_VERSION=1.27@sha256:0ecdc2a9f6156af6451080bfe3d8382a662fcc4e209608c6f919e643453514c1
FROM golang:${GO_VERSION} as build
ARG IMAGE_TAG

COPY . /src/loki
WORKDIR /src/loki
RUN go env GOARCH > /goarch
RUN make clean && make GOARCH=$(cat /goarch) BUILD_IN_CONTAINER=true GOEXPERIMENT=boringcrypto IMAGE_TAG=${IMAGE_TAG} loki-canary-boringcrypto

FROM gcr.io/distroless/base-nossl:debug
COPY --from=build /src/loki/cmd/loki-canary-boringcrypto/loki-canary-boringcrypto /usr/bin/loki-canary
SHELL [ "/busybox/sh", "-c" ]
RUN ln -s /busybox/sh /bin/sh
ENTRYPOINT [ "/usr/bin/loki-canary" ]
