# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 6: Monitor and Alert High AWS Billing Using AWS Lambda, Boto3, and SNS
Objective: In this assignment, we will create an automated alerting mechanism for when your AWS billing exceeds a certain threshold.

### Task: Set up a Lambda function to check your AWS billing amount daily, and if it exceeds a specified threshold, send an alert via SNS.
#### Step 1: Prerequisite: Enable Billing Metrics
1. AWS billing metrics are not enabled by default.
2. Enable Billing Alerts
  1. Open: `AWS Console → Billing and Cost Management`
  2. Navigate to: `Billing Preferences`
  3. Enable: `Receive Billing Alerts`
  4. Click: `Save Preferences`

#### Step 1: SNS Setup
1. Navigate to the SNS dashboard and create a new topic.
  1. Open: `AWS Console → SNS`
  2. Click: `Create Topic`
  3. Topic Configuration: `Type > Standard`, `Name > AWSBillingAlerts`
  4. Copy Topic ARN: `arn:aws:sns:ap-south-1:663130434850:AWSBillingAlerts` <img width="1980" height="338" alt="image" src="https://github.com/user-attachments/assets/45c7f8aa-1469-4246-b766-e328ff30df65" />
2. Subscribe your email to this topic.
  1. Inside the SNS Topic: `Create Subscription`
  2. Subscription Details: `Protocol > Email`, `Endpoint > Email Address` <img width="1996" height="1098" alt="image" src="https://github.com/user-attachments/assets/c2bffbe6-5e0e-403b-ae2a-c89804c86de9" />
  3. Confirm Subscription by confirming the email received. <img width="1494" height="792" alt="image" src="https://github.com/user-attachments/assets/ce576bca-ccfa-489c-b55c-e23f96df5314" /> <img width="1986" height="1018" alt="image" src="https://github.com/user-attachments/assets/cb0d8feb-ae67-4d3c-a1e6-be126aa23b60" />

#### Step 2: Create IAM Role for Lambda
1. In the IAM dashboard, create a new role for Lambda.
2. Attach policies that allow reading CloudWatch metrics and sending SNS notifications.
  1. Open: `IAM → Roles`
  2. Click: `Create Role`
  3. Trusted Entity: `AWS Service`, `Lambda`
  4. Attach Permissions: `CloudWatchReadOnlyAccess`, `AmazonSNSFullAccess`, `AWSLambdaBasicExecutionRole`
  5. Give a role name and create: `LambdaBillingMonitorRole`
<img width="1984" height="1300" alt="image" src="https://github.com/user-attachments/assets/17de2132-c724-4f17-9ecd-5d0bbdd6dd2c" />

#### Step 3: Create Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created in the previous step. <img width="1998" height="698" alt="image" src="https://github.com/user-attachments/assets/ba52de4c-bb11-45da-9cf3-c5c2ffe8d206" />
2. Write the Boto3 Python script to:
     1. Initialize boto3 clients for CloudWatch and SNS.
     2. Retrieve the AWS billing metric from CloudWatch.
     3. Compare the billing amount with a threshold (e.g., $50).
     4. If the billing exceeds the threshold, send an SNS notification.
     5. Print messages for logging purposes.
```
import boto3
from datetime import datetime, timedelta

cloudwatch = boto3.client(
    'cloudwatch',
    region_name='us-east-1'
)

sns = boto3.client('sns')

THRESHOLD = 50

TOPIC_ARN = 'arn:aws:sns:ap-south-1:663130434850:AWSBillingAlerts'

def lambda_handler(event, context):

    end_time = datetime.utcnow()
    start_time = end_time - timedelta(days=1)

    response = cloudwatch.get_metric_statistics(
        Namespace='AWS/Billing',
        MetricName='EstimatedCharges',
        Dimensions=[
            {
                'Name': 'Currency',
                'Value': 'USD'
            }
        ],
        StartTime=start_time,
        EndTime=end_time,
        Period=86400,
        Statistics=['Maximum']
    )

    datapoints = response['Datapoints']

    if not datapoints:
        print("No billing data found")
        return

    current_charge = max(
        point['Maximum']
        for point in datapoints
    )

    print(
        f"Current Estimated Charge: ${current_charge}"
    )

    if current_charge > THRESHOLD:

        message = (
            f"AWS Billing Alert!\n\n"
            f"Current Cost: ${current_charge}\n"
            f"Threshold: ${THRESHOLD}"
        )

        sns.publish(
            TopicArn=TOPIC_ARN,
            Subject='AWS Billing Alert',
            Message=message
        )

        print(
            "Billing alert sent."
        )

    else:

        print(
            "Billing below threshold."
        )

    return {
        'statusCode': 200,
        'current_charge': current_charge
    }
```
