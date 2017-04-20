FROM quay.io/aptible/nodejs:v6.9.x
MAINTAINER EpiphanyMachine@gmail.com

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl \
    apt-transport-https \
    python-software-properties \
    software-properties-common

# Add PGP keys to verify packages (psql, heroku)
RUN (curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add -) \
    && (curl -L https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -)

# Add repositories
RUN add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" \
    && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list

# Add install packages
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    google-chrome-unstable \
    heroku

# Install the aptible toolbelt https://www.aptible.com/support/toolbelt/#download-ubuntu
RUN curl -o aptible-cli.deb https://omnibus-aptible-toolbelt.s3.amazonaws.com/aptible/omnibus-aptible-toolbelt/master/132/pkg/aptible-toolbelt_0.8.4%2B20170125114552%7Eubuntu.16.04-1_amd64.deb \
    && dpkg -i aptible-cli.deb \
    && apt-get install -f \
    && rm aptible-cli.deb

# Cleanup install items
RUN rm -rf /var/lib/apt/lists/*
