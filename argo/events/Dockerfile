FROM golang:1.18 as builder

WORKDIR /workspace

COPY go.mod go.mod
COPY go.sum go.sum

RUN go mod download

COPY eventsources/ eventsources/
COPY logging/ logging/
COPY ml_metadata/ ml_metadata/

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -o run_completion_eventsource_server eventsources/run_completion/cmd/main.go

FROM gcr.io/distroless/static:nonroot
WORKDIR /
COPY --from=builder /workspace/run_completion_eventsource_server .
USER 65532:65532

ENTRYPOINT ["/run_completion_eventsource_server"]
