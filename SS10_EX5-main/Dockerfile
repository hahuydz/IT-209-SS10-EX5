FROM alpine:3.18

WORKDIR /app

RUN apk add --no-cache curl

# Mock backend application listening or running
CMD ["sh", "-c", "echo 'Backend service started on port 8080' && sleep infinity"]
