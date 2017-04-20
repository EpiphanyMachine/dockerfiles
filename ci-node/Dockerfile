FROM node:boron
MAINTAINER EpiphanyMachine@gmail.com

# Install Chromium
RUN set -xe \
    && curl -SL https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable \
    && rm -rf /var/lib/apt/lists/*

# Define working directory.
WORKDIR /data
