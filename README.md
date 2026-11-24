# HavDox DevOps Pipeline

Centralized, dynamic, and reusable CI/CD deployment workflows for the **HavDox-DriveFlow** infrastructure on AWS.

## Workflows Included

### 1. `deploy_ec2.yml`
Reusable workflow to deploy applications onto AWS EC2 instances over secure SSH with optional dynamic AWS Security Group IP whitelisting and auto-revocation.

#### Usage Example:
```yaml
jobs:
  deploy-stg:
    uses: HavDox-DriveFlow/havdox-devops-pipeline/.github/workflows/deploy_ec2.yml@main
    with:
      environment: STG
      target_host: ${{ vars.STG_HOST_IP }}
      target_user: ubuntu
      deploy_cmd: 'cd /opt/havdox && git checkout stg && git pull && docker compose up -d --build'
      aws_sg_id: ${{ vars.STG_AWS_SG_ID }}
      aws_region: us-east-1
    secrets: inherit
```

#### Secrets Required in Caller Repository:
- `SSH_PRIVATE_KEY`: Private SSH key matching the public key authorized on the target EC2 machine.
- `AWS_ACCESS_KEY_ID`: *(Optional)* If using dynamic security group ingress whitelisting.
- `AWS_SECRET_ACCESS_KEY`: *(Optional)* If using dynamic security group ingress whitelisting.
