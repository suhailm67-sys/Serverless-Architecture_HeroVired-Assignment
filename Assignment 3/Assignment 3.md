# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 3: Monitor Unencrypted S3 Buckets Using AWS Lambda and Boto3
Objective: In this assignment, we will enhance AWS security posture by setting up a Lambda function that detects any S3 bucket without server-side encryption.

### Task: Automate the detection of S3 buckets that don't have server-side encryption enabled.
#### Step 1: S3 Setup
1. Navigate to the S3 dashboard and create a few buckets. Ensure that a couple of them don't have server-side encryption enabled.
<img width="1970" height="660" alt="image" src="https://github.com/user-attachments/assets/e89f6815-673b-4801-a93d-3cab1664f841" />
<img width="1966" height="592" alt="image" src="https://github.com/user-attachments/assets/157a4e4f-62be-4f46-8192-ab415b159924" />
<img width="1984" height="694" alt="image" src="https://github.com/user-attachments/assets/38fb0c36-bbe2-4364-86ac-733c5a294d45" />

#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda and attach the `AmazonS3ReadOnlyAccess`, `AWSLambdaBasicExecutionRole` policy to this role. <img width="1958" height="1222" alt="image" src="https://github.com/user-attachments/assets/2d09f29b-ee65-45d8-bd23-c59866750a4a" />

#### Step 3: Lambda Function
1. 
