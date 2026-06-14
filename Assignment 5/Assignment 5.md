# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 5: Auto-Tagging EC2 Instances on Launch Using AWS Lambda and Boto3
Objective: In this assignment, we will automate the tagging of EC2 instances as soon as they are launched, ensuring better resource tracking and management.

### Task: Automatically tag any newly launched EC2 instance with the current date and a custom tag.
#### Step 1: EC2 Setup
1. Ensure you have the capability to launch EC2 instances.

#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda and attach the `AmazonEC2FullAccess` `AWSLambdaBasicExecutionRole` policy to this role. <img width="1958" height="1224" alt="image" src="https://github.com/user-attachments/assets/26c610a0-3985-4e42-ba72-07ab07c4402f" />

#### Step 3: Create Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created in the previous step. <img width="1942" height="720" alt="image" src="https://github.com/user-attachments/assets/c5dd53d6-08e1-4577-93bf-9a8dfa5a7bc5" />
2. Write the Boto3 Python script to:
     1. Initialize a boto3 EC2 client.
     2. Retrieve the instance ID from the event.
     3. Tag the new instance with the current date and another tag of your choice.
     4. Print a confirmation message for logging purposes.
```
import boto3
from datetime import datetime

ec2 = boto3.client('ec2')

def lambda_handler(event, context):

    instance_id = event['detail']['instance-id']

    current_date = datetime.utcnow().strftime(
        '%Y-%m-%d'
    )

    ec2.create_tags(
        Resources=[instance_id],
        Tags=[
            {
                'Key': 'LaunchDate',
                'Value': current_date
            },
            {
                'Key': 'Environment',
                'Value': 'Development'
            }
        ]
    )

    print(
        f"Tagged instance {instance_id}"
    )

    return {
        'statusCode': 200,
        'instance_id': instance_id
    }
```
#### Step 4: CloudWatch Events:
1. Set up a CloudWatch Event Rule to trigger the EC2 instance launch event. <img width="1456" height="1402" alt="image" src="https://github.com/user-attachments/assets/1e4dc649-450f-4591-a902-d825ef1f8d23" />
2.  Attach the Lambda function as the target. <img width="1458" height="1450" alt="image" src="https://github.com/user-attachments/assets/1e9520bc-cea9-4a03-a27d-1f21055881ce" />
<img width="1954" height="1234" alt="image" src="https://github.com/user-attachments/assets/314b2e91-0f93-48d1-9d7d-88a000f491f4" />
