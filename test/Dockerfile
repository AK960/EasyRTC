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

# Install app dependencies
# Wildcard installs bot package.json and package-lock.json
COPY package*.json $APPDIR/
RUN npm install

### Option 1 - Install EasyRTC from npm-registry
# RUN npm install open-easyrtc@latest && \
#     npm install --production
#
# Copy application files
# COPY . $APPDIR
#
# Replace require-paths is JS files to ensure open-easyrtc is installed from node_modules
# RUN find $APPDIR -name "*.js" -exec sed -i "s|easyrtc = require(\"../\")|easyrtc = require(\"open-easyrtc\")|g" {} +

### Option 2 - Install EasyRTC from GitHub
RUN git clone https://github.com/open-easyrtc/open-easyrtc.git && \
    cd open-easyrtc && \
    npm install 

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