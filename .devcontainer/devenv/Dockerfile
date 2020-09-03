FROM php:fpm

# Configure apt
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils 2>&1

# Install required packages and tools
RUN apt-get update; \
	apt-get -y install git procps lsb-release dialog unzip nano mariadb-client ssl-cert wget

# Install NodeJS 10
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - \
	&& apt-get install -y nodejs

# Install Gulp NPM Package Globally
RUN npm install -g gulp

# Install composer and wp-cli from factoery images
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY --from=wordpress:cli-php7.2 /usr/local/bin/wp /usr/local/bin/wp

# Configure new user
ARG USERNAME=developer
RUN useradd -m $USERNAME
ENV HOME /home/$USERNAME

# [Optional] Add sudo support
RUN apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

# Set the default shell to bash instead of sh
ENV SHELL /bin/bash

# Set the default user
USER $USERNAME