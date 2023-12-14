# Deploy to Environment Action

## Description
This GitHub Action is designed to automate the deployment of a repository to different environments using AWS CloudFormation. It handles role assumption, CloudFormation template validation, writing configuration to AWS SSM, and deploying the stack with specific image tags and accounting tags.

## Inputs

### `environment`
**Required** The environment where the deployment will occur. It must be one of the following:
- `biscred-dev`
- `biscred-prod`
- `bisnow`
- `vapor`


### `image-tag`
**Required** The tag of the Docker image that will be deployed. This tag is used to set the `ImageVersion` parameter in the CloudFormation template.

### `accounting-tag`
**Required** A tag for accounting purposes, attached to the CloudFormation stack. Default: `'none'`.

## Workflow Steps

1. **Assume Role for Environment**: Assumes the appropriate AWS role for the specified environment.

2. **Check CloudFormation Template**: Validates the CloudFormation template using `stelligent/cfn_nag` for potential security issues or misconfigurations.

3. **Write SQS Config File to SSM**: Writes an SQS configuration file to AWS SSM Parameter Store, optionally encoding it in base64.

4. **Escape JSON String**: Safely escapes the `accounting-tag` input to ensure it is JSON-compatible.

5. **Deploy to AWS CloudFormation**: Deploys the application to AWS using the specified CloudFormation template, image tag, and accounting tag.

## Usage

To use this action in your GitHub workflow, define a job that includes this action and provide the necessary inputs. Here's an example:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2
      - name: Deploy to Environment
        uses: bisnow/github-actions-deploy-to-environment
        with:
          environment: 'biscred-dev'
          image-tag: 'v1.2.3'
          accounting-tag: 'project123'
```
Replace biscred-dev with the environment you wish to assume the role for.

## Additional Notes

The paths to CloudFormation templates and SSM parameters should be valid and reflect the structure of your project.
Test this action in a controlled environment, such as a development or staging environment, before using it in production.
Contributing

For suggestions or contributions, please open an issue or a pull request in the repository.