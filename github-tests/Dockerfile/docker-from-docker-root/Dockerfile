FROM golang:1.27-alpine@sha256:4c9fe60190a2a3350ddc51de80d0224b8a6698d12bdfc999fee45ea9d6c46dbc as builder

# Install certs, git, and mercurial
# RUN apk add --no-cache ca-certificates git build-base

WORKDIR /workspace

# Copy go.mod etc and download dependencies (leverage docker layer caching)
COPY go.mod go.mod
# COPY go.sum go.sum
ENV GO111MODULE=on
RUN go mod download

# Copy source code over
COPY ./ .

# Build
RUN go build -o foo ./

# Refer to https://github.com/GoogleContainerTools/distroless for more details
FROM gcr.io/distroless/static:nonroot@sha256:963fa6c544fe5ce420f1f54fb88b6fb01479f054c8056d0f74cc2c6000df5240 
WORKDIR /
COPY --from=builder /workspace/foo .
USER nonroot:nonroot

ENTRYPOINT [ "/foo" ]