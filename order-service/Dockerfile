# Step 1: Build the Go binary
FROM golang:1.16 AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy all files into the container
COPY . .

# Install Go dependencies
RUN go mod tidy

# Build the Go app (ensure it's compiled for Linux and without dependencies)
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .

# Step 2: Use a minimal image to run the application
FROM alpine:latest

# Install necessary certificates (required for making HTTPS requests)
RUN apk --no-cache add ca-certificates

# Set the working directory inside the container
WORKDIR /root/

# Copy the compiled Go binary from the builder stage
COPY --from=builder /app/app .

# Expose the port your app will run on
EXPOSE 4000

# Command to run the Go binary
CMD ["./app"]