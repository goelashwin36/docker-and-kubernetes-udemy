# Dockerizing Node App

The project aims to create a simple node/express application and learn how to run it on docker containers.

Steps Followed:

1. Create a simple express application

```bash
# Install express-generator
npm install -g express-generator

# Make a new express application
express --view=ejs nodeapp

# Install dependencies
cd nodeapp
npm install
```

2. Create a Dockerfile

The best way to get started with Dockerfile is execute the commands in read in a docker container and then write that in a Dockerfile.

So the commands that we might execute are:

```bash
# Running docker container and mounting the current directory in the docker container
# We also need to map a docker container port to a host port
docker run -it -v ${PWD}:/app -p 8080:3000 alpine sh

cd app

# Installing dependencies
apk add --update nodejs nodejs-npm
npm install

# Starting the server
npm start
```

Now, inside the docker container, the node server is running on the port 3000. Since we have mapped the same to the port 8080, we should be able to open localhost:8080 in the browser.

Let's try converting the same to a Dockerfile

```Dockerfile
# Choosing a Base image
FROM node:alpine

# Instructions

# Starting with directory /app
WORKDIR /app

# Copying files in current directory to docker container
COPY . .
RUN npm install

EXPOSE 3000

# Startup command
CMD ["npm", "start"]
```

The first part of `Dockerfile` is choosing the `best image`. Note that instead of `alpine` we are using `node:alpine` since it will have nodejs preinstalled and we don't need to install it explicitly as in while choosing alpine.

Important Notes:

1. Suppose the container is running and if we make changes in the local files then it is not going to reflect inside docker container. Notice that in the Dockerfile we copied the current folder to the docker container. But no reference has been made between both.
2. The `package.json` file has all the dependencies. Everytime we build the Dockerfile, since we have made changes in the code, all the depedencies get installed again. However, we might not want to perform this unless we have made changes in the dependencies. We can simply use the build cache and save our time. Thus,

```Dockerfile
# Choosing a Base image
FROM node:alpine

# Instructions

# Starting with directory /app
WORKDIR /app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 3000

CMD [ "npm", "start" ]
```

Now, in the following `Dockerfile`, since we are copying the `package.json` and installing dependencies beforehand therefore, if no changes have been made to the dependencies then docker will simply use previous build cache and save our time.
