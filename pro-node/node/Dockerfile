FROM node:18

# Create app directory
WORKDIR /usr/src/app

RUN apt-get install && apt-get update && \
    apt-get install -y vim

RUN npm install -g @angular/cli

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
# COPY package*.json ./

# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
# COPY . .

CMD [ "node", "server.js" ]