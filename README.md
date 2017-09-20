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

## Usage

__1. Launch the CloudFormation stack on your account__

```bash
aws cloudformation deploy \
  --template-file stack.yml \
  --stack-name ecs-metadata \
  --capabilities CAPABILITY_IAM
```

__2. Bootstrap the Bucket__

Because the Lambda only gets called when there is an update to the metadata your endpoint will start out with no data, unless you manually bootstrap it with the current metadata. You can copy the metadata from the example endpoint above, or manually construct the metadata based on the [AMI info in the official documentation](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html).

Go to the Lambda dashboard, select the lambda function that was created, and click the "Test" button. The first time you click it a dialog will appear asking you to configure the Test Event. Choose the "SNS" template, and substitute the `message` property with a properly escaped JSON document. For example:

```json
{
  "Records": [
    {
      "EventVersion": "1.0",
      "EventSubscriptionArn": "arn:aws:sns:EXAMPLE",
      "EventSource": "aws:sns",
      "Sns": {
        "SignatureVersion": "1",
        "Timestamp": "1970-01-01T00:00:00.000Z",
        "Signature": "EXAMPLE",
        "SigningCertUrl": "EXAMPLE",
        "MessageId": "95df01b4-ee98-5cb9-9903-4c221d41eb5e",
        "Message": "{\"ECSAgent\":{\"ReleaseVersion\":\"1.14.4\"},\"ECSAmis\":[{\"ReleaseVersion\":\"2017.03.f\",\"AgentVersion\":\"1.14.4\",\"ReleaseNotes\":\"This AMI includes the latest ECS agent 2017.03.f\",\"OsType\":\"linux\",\"OperatingSystemName\":\"Amazon Linux\",\"Regions\":{\"ap-northeast-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-b743bed1\"},\"ap-southeast-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-9d1f7efe\"},\"ap-southeast-2\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-c1a6bda2\"},\"ca-central-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-b677c9d2\"},\"eu-central-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-0460cb6b\"},\"eu-west-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-8fcc32f6\"},\"eu-west-2\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-cb1101af\"},\"us-east-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-9eb4b1e5\"},\"us-east-2\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-1c002379\"},\"us-west-1\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-4a2c192a\"},\"us-west-2\":{\"Name\":\"amzn-ami-2017.03.f-amazon-ecs-optimized\",\"ImageId\":\"ami-1d668865\"}}}]}",
        "Type": "Notification",
        "UnsubscribeUrl": "EXAMPLE",
        "TopicArn": "arn:aws:sns:EXAMPLE",
        "Subject": "TestInvoke"
      }
    }
  ]
}
```

Then click "Save and Test". This will invoke the Lambda function as if it had been called by the ECS metadata SNS topic, and save the metadata in your S3 bucket.

__3. Access the metadata in the bucket.__

Return to the CloudFormation stack that you just created and check the outputs for the stack. There will an output called "URL" which is the public URL of the bucket containing the metadata. Access this URL as you see fit!



