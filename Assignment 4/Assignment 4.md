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
