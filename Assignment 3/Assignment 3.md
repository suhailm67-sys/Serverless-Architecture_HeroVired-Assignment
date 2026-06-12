# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 3: Monitor Unencrypted S3 Buckets Using AWS Lambda and Boto3
Objective: In this assignment, we will enhance AWS security posture by setting up a Lambda function that detects any S3 bucket without server-side encryption.

### Task: Automate the detection of S3 buckets that don't have server-side encryption enabled.
#### Step 1: S3 Setup
1. Navigate to the S3 dashboard and create a few buckets. Ensure that a couple of them don't have server-side encryption enabled.
<img width="1494" height="742" alt="image" src="https://github.com/user-attachments/assets/6484637f-acf5-4369-880f-5daf6d4cfc04" />
<img width="1490" height="740" alt="image" src="https://github.com/user-attachments/assets/14a96906-045d-4a99-8745-f71779001683" />
<img width="1984" height="694" alt="image" src="https://github.com/user-attachments/assets/38fb0c36-bbe2-4364-86ac-733c5a294d45" />

#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda and attach the `AmazonS3ReadOnlyAccess`, `AWSLambdaBasicExecutionRole` policy to this role. <img width="1958" height="1222" alt="image" src="https://github.com/user-attachments/assets/2d09f29b-ee65-45d8-bd23-c59866750a4a" />

#### Step 3: Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created previous step. <img width="1986" height="770" alt="image" src="https://github.com/user-attachments/assets/3b6e0588-07ef-41e9-8f06-e1a655774e38" />
2. Write the Boto3 Python script to:
     1. Initialize a boto3 S3 client.
     2. List all S3 buckets.
     3. Detect buckets without server-side encryption.
     4. Print the names of unencrypted buckets for logging purposes.
```
import boto3
from botocore.exceptions import ClientError

s3 = boto3.client('s3')

def lambda_handler(event, context):

    unencrypted_buckets = []

    buckets = s3.list_buckets()

    for bucket in buckets['Buckets']:

        bucket_name = bucket['Name']

        try:
            s3.get_bucket_encryption(
                Bucket=bucket_name
            )

            print(
                f"{bucket_name} : Encryption Enabled"
            )

        except ClientError as e:

            error_code = e.response['Error']['Code']

            if error_code == 'ServerSideEncryptionConfigurationNotFoundError':

                unencrypted_buckets.append(
                    bucket_name
                )

                print(
                    f"UNENCRYPTED BUCKET: {bucket_name}"
                )

    print(
        f"Total Unencrypted Buckets: {len(unencrypted_buckets)}"
    )

    return {
        'statusCode': 200,
        'unencrypted_buckets': unencrypted_buckets
    }
```
#### Step 4: Manual Invocation
1. After saving your function, manually trigger it. <img width="1928" height="1422" alt="image" src="https://github.com/user-attachments/assets/bd0da3be-7d74-4c73-82ea-a58dc574f223" />
### Note:  
