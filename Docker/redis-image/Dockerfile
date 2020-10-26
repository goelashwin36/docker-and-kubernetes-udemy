# Specify a base image
FROM alpine

# Download and install dependencies
RUN apk add --update redis
RUN apk add --update gcc

# Specify a startup command
CMD ["redis-server"]