FROM debian:buster

RUN apt-get update
RUN apt-get install -y \
	curl \
	libgtk-3-0 \
	libx11-xcb1 \
	libxss1 \
	libnss3 \
	libasound2

RUN mkdir -p /home/developer
ENV HOME /home/developer
WORKDIR /home/developer

ENV ZEEBE_VERSION=0.10.0
RUN curl -L https://github.com/zeebe-io/zeebe-modeler/releases/download/v${ZEEBE_VERSION}/zeebe-modeler-${ZEEBE_VERSION}-linux-x64.tar.gz > zeebe-modeler.tgz
RUN tar xfvz zeebe-modeler.tgz

RUN export uid=1000 gid=1000 && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    chown ${uid}:${gid} -R /home/developer

RUN chown root ./zeebe-modeler-${ZEEBE_VERSION}-linux-x64/chrome-sandbox
RUN chmod 4755 ./zeebe-modeler-${ZEEBE_VERSION}-linux-x64/chrome-sandbox

COPY docker-entrypoint.sh /

USER developer
ENTRYPOINT [ "/docker-entrypoint.sh" ]
