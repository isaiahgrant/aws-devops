ARG ALPINE_VERSION=3.13
ARG NODE_VERSION=14.15.4
ARG NPM_VERSION=14.15.4
ARG PYTHON_VERSION=3.8.7
ARG PIP_VERSION=20.3.4
ARG AWS_CLI_VERSION=1.19.2
ARG AWS_SAM_CLI_VERSION=1.17.0
ARG AWS_CDK_VERSION=1.89.0

FROM alpine:${ALPINE_VERSION}

ARG ALPINE_VERSION
ARG NODE_VERSION
ARG NPM_VERSION
ARG PYTHON_VERSION
ARG PIP_VERSION
ARG AWS_CLI_VERSION
ARG AWS_SAM_CLI_VERSION

ENV PATH "$PATH:/root/.local/bin"
RUN printf "===== Installing Dependencies =====\n\n" && \
    printf " - Installing gcc - \n\n" && apk add --no-cache --virtual vpack1 build-base && \
    printf " - Installing Node and NPM - \n\n" && apk add --no-cache nodejs=${NODE_VERSION}-r0 npm=${NPM_VERSION}-r0 && \
    printf " - Installing Python - \n\n" && \
        apk --no-cache add python3=${PYTHON_VERSION}-r1 py3-pip=${PIP_VERSION}-r0 && \
        apk add --no-cache --virtual vpack2 python3-dev=${PYTHON_VERSION}-r1 && \
    printf "\n\n\n\n" && \
    printf "===== Installing AWS Tooling =====\n\n" && \
    printf " - Installing AWS CLI - \n\n" && pip install --no-cache-dir awscli==${AWS_CLI_VERSION} && \
    printf " - Installing AWS SAM CLI - \n\n" && pip install --no-cache-dir aws-sam-cli==${AWS_SAM_CLI_VERSION} && \
    printf " - Installing AWS CDK - \n\n" && npm install -g aws-cdk@${AWS_CDK_VERSION} && \
    npm cache clean -f && apk del --no-cache vpack1 vpack2

ENTRYPOINT [ "ash" ]
