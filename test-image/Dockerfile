FROM node:6-alpine

# Create reports directory
RUN mkdir -p /home/nithin/JUnit

# Create a server directory
RUN mkdir -p /home/nithin/node/tests
WORKDIR /home/nithin/node/tests

# Install app dependencies
COPY /test-image/package.json /home/nithin/node/tests
RUN npm install

# Copy test source
COPY test.js /home/nithin/node/tests
EXPOSE 9000

CMD ["npm", "test"]