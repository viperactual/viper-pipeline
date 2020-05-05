# --------------------------------------------------------------------------
# Image Setup
# --------------------------------------------------------------------------

FROM viperps/pipeline:7.2

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

COPY ./php/scripts/*.sh /tmp/

# --------------------------------------------------------------------------
# Install
# --------------------------------------------------------------------------

RUN chmod +x /tmp/*.sh

RUN bash ./packages.sh \
    && bash ./chromium.sh

USER $IMAGE_USER

WORKDIR /var/www/html
