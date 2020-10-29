# Base image
FROM node:alpine

# Instructions

# Starting with directory /app
WORKDIR /app

# Copying files in current directory to docker container
COPY . ./
RUN npm install

EXPOSE 3000

# Startup command
CMD ["npm", "start"]