# Demo .NET Core Web API CI/CD Pipeline ReadMe

AWS Cloud Formation templates for building a CI/CD Pipeline

## References
- Reference Architecture Repository:  
https://github.com/awslabs/ecs-refarch-continuous-deployment
- Reference Architecture Walk-Through:  
https://docs.aws.amazon.com/AWSGettingStartedContinuousDeliveryPipeline/latest/GettingStarted/ECS_CD_Pipeline.html

## Edit CloudFormation Templates
1. Edit **service.yaml**, **deploymentpipeline.yaml** to replace `simple-app` with `demo-netcore-api`.
2. Update `TaskDefinition` in **service.yaml** to replace `ContainerDefinitions`:
    ```yaml
    ContainerDefinitions:
        - Name: demo-netcore-api
            Image: 229130418844.dkr.ecr.eu-west-1.amazonaws.com/architecture/demo-netcore-api:latest
            Essential: true
            Memory: 512
            PortMappings:
            - ContainerPort: 80
            LogConfiguration:
            LogDriver: awslogs
            Options:
                awslogs-region: !Ref AWS::Region
                awslogs-group: !Ref LogGroup
                awslogs-stream-prefix: !Ref AWS::StackName
    ```

## Steps
1. Generate a **Personal Access Token** in GitHub.
    - Go to https://github.com/settings/tokens
    - Click **Generate New Token**
    - Description: AWS CloudFormation Access Token
    - Scopes: repo
    - Click **Generate Token**
    - Copy token to a text file
2. Create S3 Bucket with CloudFormation templates.
    - Go to https://s3.console.aws.amazon.com
    - Click **Create bucket**
    - Bucket name: demo-netcore-api-cicd
    - Region: EU (Ireland)
    - Upload **demo-netcore-api-cicd.yaml** and **templates** folder.
    - Copy URL for **demo-netcore-api-cicd.yaml**.
3. Create CloudFormation Stack.
    - Go to https://eu-west-1.console.aws.amazon.com/cloudformation
    - Click **Create Stack**
    - S3 Template URL: Paste URL for **demo-netcore-api-cicd.yaml**.
    - Stack name: demo-netcore-api-cicd
    - Repo: demo-netcore-api-cicd
    - User name: hilti-labs
    - Personal Access token: Paste token from Step 1.

