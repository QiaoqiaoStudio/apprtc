FROM debian:8

ARG GOPATH=/root/go
ARG GOBINPATH=/usr/local/go/bin
ARG WD=/usr/src/app

# Create app directory
WORKDIR $WD

# Pre-req
RUN apt update
RUN apt install curl git -y

# Copy to work dir
COPY . apprtc

# Go
RUN curl https://dl.google.com/go/go1.8.7.linux-amd64.tar.gz -o go1.8.7.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf apprtc/go1.8.7.linux-amd64.tar.gz
RUN rm -rf go1.8.7.linux-amd64.tar.gz
RUN echo "export PATH=$PATH:$GOBINPATH"  >> $HOME/.bashrc
RUN echo "export GOPATH=$GOPATH"  >> $HOME/.bashrc
RUN mkdir -p $GOPATH/src

RUN ln -s $WD/apprtc/src/collider/collider $GOPATH/src
RUN ln -s $WD/apprtc/src/collider/collidermain $GOPATH/src
RUN ln -s $WD/apprtc/src/collider/collidertest $GOPATH/src

RUN $GOBINPATH/go get collidermain
RUN $GOBINPATH/go install collidermain
# RUN $GOBINPATH/go test collider

# cert
RUN mkdir -p /cert
RUN openssl req -x509 -newkey rsa:2048 -keyout /cert/key.pem -out /cert/cert.pem -days 99999 -nodes -subj "/C=US/ST=Denial/L=Springfield/O=Dis/CN=www.example.com"

EXPOSE 8089

CMD [ "/root/go/bin/collidermain", "-port=8089", "-tls=true" ]
