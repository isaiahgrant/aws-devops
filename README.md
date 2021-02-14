# aws-devops

## Overview

### Technical

The biggest benefits of the current strategy are:

* All AWS utilities are available: AWS CLI, SAM CLI, and the AWS CDK. No more installing this tooling in your pipelines or managing multiple images.

* Since these tools require Node and Python, any projects that use those do not have to worry about using different images in other pipelines.

* The image uses Alpine as a base, serving a smaller image.

* The image is public and doesn't require a login. This was a **huge** point for usability in CI/CD pipelines.

### Usage

#### Command Line

Direct invocation (no interactive terminal):

```{bash}
docker run --rm grantdscs/aws-devops -c 'aws --version'
aws-cli/1.19.2 Python/3.8.7 Linux/4.19.104-linuxkit botocore/1.20.7
```

Interactive terminal:

```{bash}
docker run --rm -it grantdscs/aws-devops
/ # aws --version
aws-cli/1.19.2 Python/3.8.7 Linux/4.19.104-linuxkit botocore/1.20.7
```

Using AWS Profiles:

```{bash}
docker run --rm -it -v "~/.aws:~/.aws" grantdscs/aws-devops
/ # sam --version
SAM CLI, version 1.17.0
```

Using environment variables:

```{bash}
docker run --rm -it -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e AWS_SESSION_TOKEN -e AWS_DEFAULT_REGION -e AWS_DEFAULT_OUTPUT -e SAM_CLI_TELEMETRY grantdscs/aws-devops
/ # cdk --version
1.89.0 (build df7253c)
```

#### Gitlab CI/CD

```{yaml}
validate:
  image: grantdscs/aws-devops
  stage: test
  script:
    - sam validate --template cloudformation.yml
```

**Note:** You can provide AWS configuration through environment variables, roles on the runners, etc.

#### Jenkins

```{groovy}
pipeline {
    agent {
        docker { image 'grantdscs/aws-devops' }
    }
    stages {
        stage('Test') {
            steps {
                sam validate --template cloudformation.yml
            }
        }
    }
}
```

## About

In my experience I often require access to tooling around AWS for DevOps workloads. I have long been a proponent of an AWS-managed resource for this because... well why wouldn't they? With the release of the AWS CLI V2, I was hopeful that it would simplify my usage of AWS in CI/CD pipelines however it's usage still isn't quite what I often require. I have made other attempts at an image like this in the past and experienced surprising results in regards to how much of the community used it. Now that I no longer have the ability to maintain those resources, I do not like the idea of being dependent on them for my future workloads. As such this image was born. Unlike previous examples, I have taken a less targeted approach and am focusing on ease-of-delivery and maintainability as I hope to continue maintaining this over time.
