
FROM buildpack-deps:stretch

# Expose the SSH port
EXPOSE 22

RUN echo "deb http://winswitch.org/ stretch main" > /etc/apt/sources.list.d/winswitch.list \
    && curl https://winswitch.org/gpg.asc | apt-key add - \
	&& apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y \
	    xpra \
        x11-apps \
        xserver-xephyr \
        openssh-server \
        python-pip \
        net-tools \
        nano mc htop \
    && pip install netifaces \
    && apt-get clean

# Create OpenSSH privilege separation directory
RUN mkdir /var/run/sshd

# /bin/echo - full path need for correct parameters processing such "-e"
RUN /bin/echo -e "AddressFamily inet\nPasswordAuthentication yes" >> /etc/ssh/sshd_config

ENV PASS=pass
ENV USERNAME=user
RUN useradd --uid 1000 -p $(openssl passwd -1 user) user

ENV DISPLAY=":100"
RUN sed -i '1s/^/[ -z "$DISPLAY" ] \&\& export DISPLAY=":100"\n/' /etc/bash.bashrc

# Start SSH anx Xpra
CMD mkdir -p /home/user/.ssh/ && chown -R user:user /home/user \
    && /usr/sbin/sshd && rm -f /tmp/.X100-lock \
    && su user -c "xpra start $DISPLAY && sleep 1 && cp ~/.xpra/run-xpra /tmp/run-xpra \
    && cat /tmp/run-xpra | grep -v affinity > ~/.xpra/run-xpra && sleep infinity"
