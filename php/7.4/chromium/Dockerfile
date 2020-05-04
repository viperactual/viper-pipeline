# --------------------------------------------------------------------------
# Image Setup
# --------------------------------------------------------------------------

FROM viperps/pipeline:7.4

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Michael Noël <mike@viperframe.work>" \
      PHP="7.4" \
      NODE="12" \
      org.label-schema.name="viperactual/viper-pipeline" \
      org.label-schema.description=":coffee: Docker images for building and testing PHP applications with Gitlab CI." \
      org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.schema-version="1.0" \
      org.label-schema.vcs-url="https://github.com/viperactual/viper-pipeline" \
      org.label-schema.vcs-ref=$VCS_REF

# --------------------------------------------------------------------------
# Set Correct Environment Variables
# --------------------------------------------------------------------------

ENV IMAGE_USER=php
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer
ENV PATH=$HOME/.yarn/bin:$PATH
ENV GOSS_VERSION="0.3.8"
ENV PHP_VERSION=7.4

USER root

WORKDIR /tmp

# --------------------------------------------------------------------------
# Copy Install Scripts
# --------------------------------------------------------------------------

COPY ./php/scripts/*.sh /tmp/
RUN chmod +x /tmp/*.sh

# --------------------------------------------------------------------------
# Install
# --------------------------------------------------------------------------

RUN bash ./packages.sh \
    && bash ./chromium.sh

USER $IMAGE_USER

WORKDIR /var/www/html
