# --------------------------------------------------------------------------
# Image Setup
# --------------------------------------------------------------------------

FROM php:7.2-fpm

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Michael Noël <mike@viperframe.work>" \
      PHP="7.2" \
      NODE="12" \
      org.label-schema.name="viperps/viper-pipeline" \
      org.label-schema.description=":coffee: Deploy PHP applications with Docker Images and GitLab CI." \
      org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.schema-version="1.0" \
      org.label-schema.vcs-url="https://github.com/viperps/viper-pipeline" \
      org.label-schema.vcs-ref=$VCS_REF

# --------------------------------------------------------------------------
# Set Correct Environment Variables
# --------------------------------------------------------------------------

ENV IMAGE_USER=php
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer
ENV PATH=$HOME/.yarn/bin:$PATH
ENV GOSS_VERSION="0.3.8"
ENV PHP_VERSION=7.2

USER root

WORKDIR /tmp

# --------------------------------------------------------------------------
# Copy Install Scripts
# --------------------------------------------------------------------------

COPY --from=composer:1 /usr/bin/composer /usr/bin/composer
COPY ./php/scripts/*.sh /tmp/
RUN chmod +x /tmp/*.sh

# --------------------------------------------------------------------------
# Install
# --------------------------------------------------------------------------

RUN bash ./packages.sh \
    && bash ./extensions.sh \
    && bash ./node.sh \
    && adduser --disabled-password --gecos "" $IMAGE_USER && \
    echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers && \
    mkdir -p /var/www/html && \
    chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME \
    && composer global require "hirak/prestissimo:^0.3"  \
    && rm -rf ~/.composer/cache/* \
    && chown -R $IMAGE_USER:$IMAGE_USER $COMPOSER_HOME \
    && curl -fsSL https://goss.rocks/install | GOSS_VER=v${GOSS_VERSION} sh \
    && bash ./cleanup.sh

USER $IMAGE_USER

WORKDIR /var/www/html
