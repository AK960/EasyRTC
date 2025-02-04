# Base image
FROM node:14-alpine

# Install git, upgrade npm, add user
RUN apk add -U --no-cache git && \
    npm install -g npm@6 && \
    adduser -D easyrtcuser 
ENV NO_UPDATE_NOTIFIER=1

# Create app directory
ENV APPDIR=/usr/src/app
RUN mkdir -p $APPDIR
WORKDIR $APPDIR

# Install app dependencies from package.json
COPY package*.json $APPDIR/
RUN npm install

# Copy the local repository files into the Docker image
COPY . $APPDIR/open-easyrtc

# Set up the server_example directory
WORKDIR $APPDIR/open-easyrtc/server_example
RUN npm install --no-bin-links

# Create volumes outside the container for persistent storage of data
# Option 1
# VOLUME ["certs", "static", "$APPDIR/node_modules/open-easyrtc/demos"]
# Option 2
VOLUME ["certs", "static", "$APPDIR/open-easyrtc/demos"]

# Switch to non-root user
USER easyrtcuser

# Expose Ports
EXPOSE 8080 8443

# Set environmental variable
ENV SYLAPS_ENV=all

# Start EasyRTC server
ENTRYPOINT ["npm"]
CMD ["start"]