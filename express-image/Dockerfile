FROM node:6-alpine

# Create a server directory
RUN mkdir -p /home/nithin/node/app
WORKDIR /home/nithin/node/app

# Install server dependencies
COPY /express-image/package.json /home/nithin/node/app
RUN npm install

# Copy node Application
COPY app.js /home/nithin/node/app

# Open port
EXPOSE 9000

CMD ["npm", "start"]