# HomeX Vault-AWS Action

This repository contains a github action that authenticates into Vault using an App Role to return generated AWS credentials and set the profile in the environment for AWS CLI/SDKs to use. 

## Prerequisites 

1. Vault AWS App Role is set up with the account and role you will be assuming when using this action.
2. AWS secrets engine is set up to generate credentials against the specified role.
3. Role/secret id are added as secrets to the repository where this action will be used. Make sure you use the role/secret id specific to the App Role being used for your project (E.g. `${{ secrets.<project_name>_VAULT_ROLE_ID }}`)


## Inputs

| Variable | Required | Description | Default Value |
|----------|----------|-------------|---------------|
| aws-region | ✘ | The AWS Region | `us-east-1` |
| vault-address | ✓ | The address for your instance of Vault |  |
| role-id | ✓ | The secret containing the role Id for the Vault App Role |  |
| secret-id | ✓ | The secret containing secret Id for the Vault App Role |  |
| vault-path | ✓ | The path in Vault where the secrets engine generates AWS credentials |  |
| role-to-assume-arn | ✘ | The ARN for the role wanting to be assumed in AWS | `null` |
| role-duration-seconds | ✘ | The duration in seconds that the role remains active (default: 15 minutes) | `900` |
| role-skip-session-tagging | ✘ | Skip session tagging during role assumption | `true` |


## Results

The `configure-aws-credentials` action will set environment variables containing the AWS region and credentials for other Github Actions to use. The environment variables will be detected by both the AWS SDKs and the AWS CLI to determine the credentials and region to use for AWS API calls.

## Example Use

The latest version of the action can be found on [the marketplace](https://github.com/marketplace/actions/vault-aws-authentication) or use @main.

Add the following step to your workflow.
```

    - name: Vault-AWS Authentication
      uses: HomeXLabs/vault-aws-auth@v1.1.2
      with:
        aws-region: us-east-1
        vault-address: https://vault.mycompany.com:8200
        role-id: ${{ secrets.VAULT_ROLE_ID }}
        secret-id: ${{ secrets.VAULT_SECRET_ID }}
        vault-path: /aws/creds/my-project-name

```
