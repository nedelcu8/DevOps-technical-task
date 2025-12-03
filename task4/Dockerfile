# Use official Golang image for build
FROM golang:1.21 AS builder

WORKDIR /app
COPY . .

# Build the binary
RUN go mod init sampleapp && \
    go build -o sampleapp main.go

# Final lightweight image
FROM ubuntu:22.04
WORKDIR /app

# Copy binary from builder
COPY --from=builder /app/sampleapp .

ENTRYPOINT ["./sampleapp"]
