FROM quay.io/app-sre/boilerplate:image-v4.0.2 AS builder

WORKDIR /workdir
COPY go.mod go.mod
COPY go.sum go.sum
RUN go mod download
COPY . .
RUN make go-build

####
FROM registry.access.redhat.com/ubi8/ubi-minimal:8.10-1018

ENV USER_UID=1001 \
    USER_NAME=configure-goalert-operator

COPY --from=builder /workdir/build/_output/bin/* /usr/local/bin/

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}

LABEL io.openshift.managed.name="configure-goalert-operator" \
      io.openshift.managed.description="Operator to manage registering clusters with GoAlert"