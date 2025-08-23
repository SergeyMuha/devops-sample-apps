# Build stage
FROM golang:1.22-alpine AS builder

# Set working directory
WORKDIR /app

# Copy source code
COPY main.go go.mod ./

# Build the Go binary (statically linked)
RUN go build -o server .

# Final runtime stage
FROM alpine:latest

WORKDIR /app

# Copy binary from builder
COPY --from=builder /app/server .

# Copy file.p12 if you want it inside the container
COPY file.p12 .

# Expose HTTP port
EXPOSE 80

# Run the app
CMD ["./server"]
