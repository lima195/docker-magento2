FROM ubuntu:18.04

WORKDIR /usr/share/nginx/www

RUN usermod -u 1000 www-data

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Get repository and install wget and vim
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
        apt-utils

RUN apt-get update \
    && apt-get install -y build-essential \
    && apt-get install -y nodejs \
    && apt-get install -y npm \
    && apt-get install -y yarn \
    && /usr/bin/npm install -g grunt \
    && /usr/bin/npm install -g gulp \
    && /usr/bin/npm install -g bower

# Global install yarn package manager
RUN apt-get update && apt-get install -y curl apt-transport-https && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn

RUN npm install --save-dev gulp
RUN npm install --global gulp-cli

RUN npm install