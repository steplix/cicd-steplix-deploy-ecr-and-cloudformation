# cicd-deploy-ecr-and-cloudformation

Deploy an application in AWS that uses docker and cloud formation.

## Pre-requisites

Create an [AWS ECR Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html).
Create an [AWS Access credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html).

## Inputs

### `ENVIRONMENT` (**required**)

Name of the environment to deploy.

``` yaml
with:
    ENVIRONMENT: testing
```

### `AWS_CF_STACK_NAME` (**required**)

AWS cloud-formation stack name.

``` yaml
with:
    AWS_CF_STACK_NAME: ${{ github.event.repository.name }}
```

> INFO: Recommend send the repository name.

### `AWS_ECR_REPOSITORY` (**required**)

AWS ECR Repository name.

``` yaml
with:
    AWS_ECR_REPOSITORY: 'test-repository-name'
```

### `AWS_ACCESS_KEY_ID` (**required**)

AWS Access Key ID.

``` yaml
with:
    AWS_ACCESS_KEY_ID: AKIA*****BUT
```

### `AWS_SECRET_ACCESS_KEY` (**required**)

AWS Secret Access Key.

``` yaml
with:
    AWS_SECRET_ACCESS_KEY: yWFX**********aC2sCc
```

### `AWS_REGION` (**required**)

AWS Region.

``` yaml
with:
    AWS_REGION: us-east-2
```

### `CAPABILITIES`

The comma-delimited list of stack template capabilities to acknowledge. Defaults to 'CAPABILITY_IAM'.

``` yaml
with:
    CAPABILITIES: CAPABILITY_IAM,CAPABILITY_NAMED_IAM
```

### `AWS_CLOUDFORMATION_PARAMS_OVERRIDE`

Cloudformation parameters to override when deploy the application.

``` yaml
with:
    AWS_CLOUDFORMATION_PARAMS_OVERRIDE: >-
        'PARAMETER_1=value 1,'
        'PARAMETER_2=value 2,'
        'PARAMETER_3=value 3'
```

> INFO: Always replace the following parameters:\
> &emsp;1. `Environment`: The environment sended in the inputs.\
> &emsp;2. `ParameterSuffix`: The environment but capitalized.\
> &emsp;3. `EnvAwsECRImage`: The docker image upload to AWS ECR.

### `TAG`

Tag that will be placed on the docker image to upload in ECR. The default value is the Short commit SHA.

``` yaml
with:
    TAG: '1.0.0'
```

### `DISABLE_CACHE`

Disable docker cache. By default is enabled.

`Default value`: false

``` yaml
with:
    DISABLE_CACHE: false
```

## Example Usage

``` yaml
name: SomeWorkflow

on: [push]

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - id: string
        uses: steplix/cicd-steplix-deploy-ecr-and-cloudformation@v1
        with:
            ENVIRONMENT: testing
            COMMIT_MESSAGE: ${{ github.event.head_commit.message }}
            AWS_ECR_REPOSITORY: ${{ github.event.repository.name }}
            AWS_ACCESS_KEY_ID: AKIA*****BUT
            AWS_SECRET_ACCESS_KEY: yWFX**********aC2sCc
            AWS_REGION: us-east-2
```
