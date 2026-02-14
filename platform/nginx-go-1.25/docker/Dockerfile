ARG ALPINE_VERSION=3.22
ARG GOLANG_VERSION="1.25.1"

FROM alpine:${ALPINE_VERSION}

ARG UID
ARG GID
ENV UID=${UID}
ENV GID=${GID}
ARG USER
ARG GROUP
ENV USER=${USER}
ENV GROUP=${GROUP}

RUN apk add --no-cache bash \
    && addgroup -S --gid ${GID} ${GROUP} \
    && adduser -S --uid ${UID} ${USER} -s /bin/sh -G ${GROUP} ${USER} \
    && apk add --no-cache sudo \
    && chmod u+s /bin/su \
    && echo "${USER} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Install main packages
RUN apk add --no-cache \
    openrc \
    curl \
    wget \
    openssl \
    nginx \
    zip \
    nano \
    git \
    make \
    supervisor

# Install GO
RUN wget https://go.dev/dl/go1.25.1.linux-amd64.tar.gz && \
    rm -rf /usr/local/go && tar -C /usr/local -xzf go1.25.1.linux-amd64.tar.gz && \
    rm go1.25.1.linux-amd64.tar.gz
ENV PATH="${PATH}:/usr/local/go/bin"

# Configure NGINX
COPY config/nginx/nginx.conf /etc/nginx/nginx.conf
COPY config/nginx/conf.d /etc/nginx/conf.d/
RUN rm -rf /var/www/html

# Configure Supervisor
COPY config/supervisor/supervisord.conf /etc/supervisor/supervisord.conf
COPY config/supervisor/conf.d/. /etc/supervisor/conf.d/

# Create project root
RUN mkdir -p /var/www

WORKDIR /var/www

RUN chown -R ${USER}:${GROUP} /var/www /run /var/lib/nginx /var/log/nginx

USER ${USER}

# NGINX: 80 - GO: 8080
EXPOSE 80 8080

# Build the GO Application (replace 'main.go' as your requirements)
RUN go version
RUN go install github.com/air-verse/air@latest
RUN sudo cp /home/${USER}/go/bin/air /usr/local/bin/air
ENV PATH="${PATH}:/usr/local/go/bin:/go/bin:/home/${USER}/go/bin"

# Start supervisor, which manages nginx and your Go app
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]