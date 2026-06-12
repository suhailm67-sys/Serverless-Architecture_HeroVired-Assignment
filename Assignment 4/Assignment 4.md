# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 3: Automatic EBS Snapshot and Cleanup Using AWS Lambda and Boto3
Objective: In this assignment, we will automate the backup process for your EBS volumes and ensure that backups older than a specified retention period are cleaned up to save costs.

### Task: Automate the creation of snapshots for specified EBS volumes and clean up snapshots older than 30 days.
#### Step 1: EBS Setup
1. Navigate to the EC2 dashboard and identify or create an EBS volume you wish to back up. <img width="1974" height="1272" alt="image" src="https://github.com/user-attachments/assets/e214858e-0ba8-46f7-94d6-2de295e036da" />
2. Note down the volume ID. `vol-03858640d9e6f80f8` <img width="1956" height="1578" alt="image" src="https://github.com/user-attachments/assets/f9cacd86-f6df-48ea-9d8b-9ba918a8a7db" />

#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda and attach policies that allow Lambda to create EBS snapshots and delete them (`AmazonEC2FullAccess` `AWSLambdaBasicExecutionRole` for simplicity, but be more restrictive in real-world scenarios). <img width="1950" height="1230" alt="image" src="https://github.com/user-attachments/assets/2ea9bbc7-10f5-4842-9af6-e3c969e31303" />

#### Step 3: Lambda Function
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created previous step. <img width="1992" height="752" alt="image" src="https://github.com/user-attachments/assets/84983ac4-b6b2-48cf-b952-ab272efbe79d" />
2. Write the Boto3 Python script to:
     1. Initialize a boto3 EC2 client.
     2. Create a snapshot for the specified EBS volume.
     3. List snapshots and delete those older than 30 days.
     4. Print the IDs of the created and deleted snapshots for logging purposes.
```
import boto3
from datetime import datetime, timezone, timedelta

ec2 = boto3.client('ec2')

# Replace with your volume ID
VOLUME_ID = 'vol-03858640d9e6f80f8'

def lambda_handler(event, context):

    created_snapshot_ids = []
    deleted_snapshot_ids = []

    # Create Snapshot
    snapshot = ec2.create_snapshot(
        VolumeId=VOLUME_ID,
        Description=f"Automated Backup - {datetime.now()}"
    )

    snapshot_id = snapshot['SnapshotId']

    created_snapshot_ids.append(snapshot_id)

    print(f"Created Snapshot: {snapshot_id}")

    # Retention Period
    cutoff_date = datetime.now(
        timezone.utc
    ) - timedelta(days=30)

    # Find Snapshots Owned By Current Account
    snapshots = ec2.describe_snapshots(
        OwnerIds=['self']
    )

    for snap in snapshots['Snapshots']:

        start_time = snap['StartTime']

        if start_time < cutoff_date:

            snapshot_id = snap['SnapshotId']

            try:
                ec2.delete_snapshot(
                    SnapshotId=snapshot_id
                )

                deleted_snapshot_ids.append(
                    snapshot_id
                )

                print(
                    f"Deleted Snapshot: {snapshot_id}"
                )

            except Exception as e:

                print(
                    f"Failed to delete {snapshot_id}: {e}"
                )

    return {
        'statusCode': 200,
        'created_snapshots': created_snapshot_ids,
        'deleted_snapshots': deleted_snapshot_ids
    }
```
<img width="1956" height="1422" alt="image" src="https://github.com/user-attachments/assets/8a132ac9-217b-4af5-9bfd-db03704246a0" />

#### Step 4: Event Source (Bonus)
1. Attach an event source, like Amazon CloudWatch Events, to trigger the Lambda function at your desired backup frequency (e.g., every week). <img width="1990" height="1358" alt="image" src="https://github.com/user-attachments/assets/a3d9a5b6-da71-4f6c-9514-698707880cda" />
<img width="1986" height="1176" alt="image" src="https://github.com/user-attachments/assets/34159fe4-cc95-4ce2-a0f5-43b686f62e79" />
<img width="1966" height="1564" alt="image" src="https://github.com/user-attachments/assets/cec7a389-8076-487e-bca1-349ac3a91e2a" />
