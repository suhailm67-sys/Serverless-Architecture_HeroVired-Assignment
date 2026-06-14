# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 2: Automated S3 Bucket Cleanup Using AWS Lambda and Boto3
Objective: In this assignment, we will gain experience with AWS Lambda and Boto3 by creating a Lambda function that will automatically clean up old files in an S3 bucket.

### Task: Automate the deletion of files older than 30 days in a specific S3 bucket.
#### Step 1: S3 Setup
1. Navigate to the S3 dashboard and create a new bucket. <img width="1994" height="1128" alt="image" src="https://github.com/user-attachments/assets/e17f7c66-2867-4c4c-b684-94e7db9eac64" />
2. Upload multiple files to this bucket, ensuring that some files are older than 30 days (you may need to adjust your system's date temporarily for this or use old files). <img width="1970" height="1462" alt="image" src="https://github.com/user-attachments/assets/081bdd2e-11fb-4bc4-8b19-c0e8f8f2e583" /> <img width="1988" height="1456" alt="image" src="https://github.com/user-attachments/assets/6fda539e-adbc-4386-ae85-e200ed4c3a91" />

#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda by attaching the following roles `AmazonS3FullAccess` and `AWSLambdaBasicExecutionRole`. <img width="1948" height="1316" alt="image" src="https://github.com/user-attachments/assets/4e27c1c7-4d77-406d-9833-273a1b254d4f" />

#### Step 3: Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created previous step. <img width="1992" height="850" alt="image" src="https://github.com/user-attachments/assets/66c15a68-da10-4b97-8615-9fadb82867d9" />
2. Write the Boto3 Python script to:
     1. Initialize a boto3 S3 client.
     2. List objects in the specified bucket.
     3. Delete objects older than 30 days.
     4. Print the names of deleted objects for logging purposes.
```
import boto3
from datetime import datetime, timezone, timedelta

s3 = boto3.client('s3')

BUCKET_NAME = 's3-cleanup-demo-yourname'

def lambda_handler(event, context):

    days_old = 30

    cutoff_date = datetime.now(timezone.utc) - timedelta(days=days_old)

    response = s3.list_objects_v2(
        Bucket=BUCKET_NAME
    )

    deleted_files = []

    if 'Contents' in response:

        for obj in response['Contents']:

            object_key = obj['Key']
            last_modified = obj['LastModified']

            if last_modified < cutoff_date:

                s3.delete_object(
                    Bucket=BUCKET_NAME,
                    Key=object_key
                )

                deleted_files.append(object_key)

                print(
                    f"Deleted: {object_key}"
                )

    print(
        f"Total Deleted Files: {len(deleted_files)}"
    )

    return {
        'statusCode': 200,
        'deleted_files': deleted_files
    }
```
#### Step 4: Manual Invocation
1. After saving the function, manually trigger it. <img width="2820" height="1540" alt="image" src="https://github.com/user-attachments/assets/b14d8e26-9098-488d-ad07-2ee0a88dc4e7" />
2. Go to the S3 dashboard and confirm that only files newer than 30 days remain. <img width="1980" height="792" alt="image" src="https://github.com/user-attachments/assets/f8310e7d-649e-4927-a8d2-9ba2b690a46f" />
3. Check in CloudWatch logs the files which were deleted. <img width="2010" height="996" alt="image" src="https://github.com/user-attachments/assets/2c2ac0e0-0554-4643-a7d9-87680bc38629" />

### Architecture Overview
<img width="470" height="660" alt="image" src="https://github.com/user-attachments/assets/92ddcc43-295d-40a4-b8d3-5d1dd202f230" />

