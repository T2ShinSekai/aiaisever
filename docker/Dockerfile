FROM node:latest

ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY

COPY startup.sh /opt/startup.sh

RUN apt-get update &&\
    apt-get install -y \
    python3-pip \
    jq && \
    pip3 install awscli --upgrade --user && \
    pip3 install yq && \
    apt-get install -y awscli && \
    npm install -g serverless serverless-offline &&\
    sls config credentials --provider aws --key $AWS_ACCESS_KEY_ID --secret $AWS_SECRET_ACCESS_KEY && \
    mkdir /app && \
    chmod 777 /opt/startup.sh && \
    apt-get install -y vim

WORKDIR /app

EXPOSE 3000
CMD [ "/bin/bash" ]