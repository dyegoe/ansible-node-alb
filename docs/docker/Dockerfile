FROM docker:dind

RUN apk --update upgrade && \
    apk --update add git \
                     python \
                     py-pip \
                     openssl \
                     ca-certificates \
                     openssh-client \
                     mariadb-client \
                     mariadb-client-libs \
                     bash \
                     bash-completion && \
    apk --update add --virtual build-dependencies-common \
                               python-dev \
                               libffi-dev \
                               build-base && \
    apk --update add --virtual build-dependencies-openssl \
                               openssl-dev && \
    pip install --upgrade pip cffi && \
    pip install 'ansible>=2.5.0,<2.6.0' \
                'docker-py' \
                'boto' \
                'boto3' \
                'botocore' \
                'awscli' && \
    apk del build-dependencies-openssl && \
    apk --update add --virtual build-dependencies-mariadb \
                               libressl-dev \
                               mariadb-dev && \
    pip install 'MySQL-python' && \
    apk del build-dependencies-mariadb && \
    apk del build-dependencies-common && \
    rm -rf /var/cache/apk/*

COPY source /tmp/source

RUN adduser -D -u 1000 -h /home/ansible -s /bin/bash ansible && \
    cp /tmp/source/.bashrc /home/ansible/.bashrc && \
    chown -R ansible:ansible /home/ansible/.bashrc && \
    mv /tmp/source/.bashrc /root/.bashrc && \
    chown -R root:root /root/.bashrc && \
    sed -i 's?/bin/ash?/bin/bash?' /etc/passwd && \
    sed -i 's?root:x:0:root?root:x:0:root,ansible?' /etc/group && \
    mkdir /etc/bash_completion.d/ && \
    mv /tmp/source/docker /etc/bash_completion.d/docker && \
    echo "complete -C '/usr/bin/aws_completer' aws" > /etc/bash_completion.d/aws && \
    rm -rf /tmp/source