# Build stage
FROM golang:1.22-alpine AS builder

# Set working directory
WORKDIR /app

COPY go.mod ./
RUN go mod download

# Copy the rest of the code
COPY . .

# Build the Go binary (statically linked)
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest AS certs
RUN apk add --no-cache ca-certificates


# Final runtime stage
FROM scratch
WORKDIR /app

# Copy binary from builder
COPY --from=builder /app/main .
COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
# Copy file.p12 if you want it inside the container
COPY file.p12 .

# Expose HTTP port
EXPOSE 80

# Run the app
CMD ["./main"]
