# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 7: Archive Old Files from S3 to Glacier Using AWS Lambda and Boto3
Objective: In this assignment, we will archival of files older than a certain age from an S3 bucket to Amazon Glacier for cost-effective storage.

### Task: Automatically move files in an S3 bucket older than 6 months to Glacier storage class.

#### Step 1: Create an S3 Bucket
1. Navigate to the S3 dashboard and create a bucket.
  1. Open: `AWS Console > S3` and `Create Bucket` 
  2. Upload files to the bucket. <img width="1972" height="1470" alt="image" src="https://github.com/user-attachments/assets/d168554b-5dfa-4c5e-9290-e5e2fd80e9ae" />

#### Step 2: Create IAM Role
1. In the IAM dashboard, create a new role for Lambda and attach the `AmazonS3FullAccess` `AWSLambdaBasicExecutionRole` policy to this role. <img width="1950" height="1204" alt="image" src="https://github.com/user-attachments/assets/5ebd3a9a-324c-4659-8378-5880b2530306" />

#### Step 3: Create Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created in the previous step `LambdaS3GlacierRole`. <img width="1968" height="580" alt="image" src="https://github.com/user-attachments/assets/caf29b40-aed1-4025-ac5c-09c3d3aab1d9" />
2. Write the Boto3 Python script to:
     1. List objects in the S3 bucket.
     2. Identify files older than 6 months.
     3. Change the storage class of identified files to Glacier.
     4. Log the archived files.
```
import boto3
from datetime import datetime, timezone, timedelta

s3 = boto3.client('s3')

BUCKET_NAME = 'glacier-archive-demo-suhail'

RETENTION_DAYS = 180

def lambda_handler(event, context):

    cutoff_date = (
        datetime.now(timezone.utc)
        - timedelta(days=RETENTION_DAYS)
    )

    archived_files = []

    response = s3.list_objects_v2(
        Bucket=BUCKET_NAME
    )

    if 'Contents' not in response:

        print("Bucket is empty")

        return {
            'statusCode': 200
        }

    for obj in response['Contents']:

        object_key = obj['Key']

        last_modified = obj['LastModified']

        if last_modified < cutoff_date:

            s3.copy_object(
                Bucket=BUCKET_NAME,
                Key=object_key,
                CopySource={
                    'Bucket': BUCKET_NAME,
                    'Key': object_key
                },
                StorageClass='GLACIER',
                MetadataDirective='COPY'
            )

            archived_files.append(
                object_key
            )

            print(
                f"Archived: {object_key}"
            )

    print(
        f"Total Archived: {len(archived_files)}"
    )

    return {
        'statusCode': 200,
        'archived_files': archived_files
    }
```
#### Step 4: Test the code manually
1. Manually trigger the Lambda function or set it to run periodically. <img width="1948" height="1590" alt="image" src="https://github.com/user-attachments/assets/05babc87-9fb4-4aea-ad53-a501e48d9b78" />
2. Confirm that older files in the S3 bucket are moved to the Glacier storage class. <img width="1874" height="1472" alt="image" src="https://github.com/user-attachments/assets/1d876c97-10cd-44bb-9bde-63b6aba49c48" />
<img width="1976" height="368" alt="image" src="https://github.com/user-attachments/assets/6ddd7250-7fce-4248-9763-537ad4857974" />
<img width="1972" height="390" alt="image" src="https://github.com/user-attachments/assets/2965a58a-254b-4a7f-94ef-a3d5691d9ebf" />

#### Step 5: Verify CloudWatch Logs
1. Open `Lambda → Monitor`
2. Click `View CloudWatch Logs`
3. Check the logs: <img width="2012" height="1176" alt="image" src="https://github.com/user-attachments/assets/1da78bf6-5daf-431e-abfb-e9aae2d690da" />

#### Architecture Overview: 
<img width="466" height="772" alt="image" src="https://github.com/user-attachments/assets/648904ae-d40b-4c80-abb4-bfbae24f36f7" />
