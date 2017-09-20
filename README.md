# ECS AMI Metadata Endpoint

This is a CloudFormation template that helps you subscribe to the ECS AMI Metadata updates and save them in an S3 bucket that hosts a website endpoint. 

For an example of the finished result see:

http://ecs-metadata.s3-website-us-east-1.amazonaws.com/

Example Metadata:

```json
{
  "ECSAgent": {
    "ReleaseVersion": "1.14.4"
  },
  "ECSAmis": [{
    "ReleaseVersion": "2017.03.f",
    "AgentVersion": "1.14.4",
    "ReleaseNotes": "This AMI includes the latest ECS agent 2017.03.f",
    "OsType": "linux",
    "OperatingSystemName": "Amazon Linux",
    "Regions": {
      "ap-northeast-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-b743bed1"
      },
      "ap-southeast-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-9d1f7efe"
      },
      "ap-southeast-2": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-c1a6bda2"
      },
      "ca-central-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-b677c9d2"
      },
      "eu-central-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-0460cb6b"
      },
      "eu-west-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-8fcc32f6"
      },
      "eu-west-2": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-cb1101af"
      },
      "us-east-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-9eb4b1e5"
      },
      "us-east-2": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-1c002379"
      },
      "us-west-1": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-4a2c192a"
      },
      "us-west-2": {
        "Name": "amzn-ami-2017.03.f-amazon-ecs-optimized",
        "ImageId": "ami-1d668865"
      }
    }
  }]
```

### Launching on your own account

```bash
aws cloudformation deploy \
  --template-file stack.yml \
  --stack-name ecs-metadata \
  --capabilities CAPABILITY_IAM
```
