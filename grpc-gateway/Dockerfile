# Dockerfile for gRPC Go
FROM golang:latest

# Install grpc
RUN go get -u google.golang.org/grpc
RUN go get -u github.com/golang/protobuf/protoc-gen-go
RUN go get -u github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway
RUN go get -u github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2
RUN go get -u google.golang.org/protobuf/cmd/protoc-gen-go
RUN go get -u google.golang.org/grpc/cmd/protoc-gen-go-grpc
RUN go get -u github.com/grpc-ecosystem/grpc-gateway/v2/runtime

ENV PATH=$PATH:$GOPATH/bin

WORKDIR /app
COPY . .

ENTRYPOINT go run grpc-gateway/main.go
