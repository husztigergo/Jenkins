FROM alpine:3.14

RUN apk add openssh && \
    apk add openrc && \
    apk add --no-cache \
        python3 \
        py3-pip  && \
    pip3 install --upgrade pip && \
    pip3 install --no-cache-dir \
        awscli && \
    rm -rf /var/cache/apk/* && \
    apk add mysql mysql-client

RUN addgroup -S remote_user && adduser -S remote_user -G remote_user && \
    printf "1234\n1234" | passwd remote_user && \
    mkdir -p /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh

COPY remote-host.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user && \
    chmod 600 /home/remote_user/.ssh/authorized_keys

RUN /usr/bin/ssh-keygen -A

RUN rc-update add sshd

CMD /usr/sbin/sshd -D
