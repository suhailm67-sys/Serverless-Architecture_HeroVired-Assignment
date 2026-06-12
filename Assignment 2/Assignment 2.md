# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 2: Automated S3 Bucket Cleanup Using AWS Lambda and Boto3
Objective: In this assignment, we will gain experience with AWS Lambda and Boto3 by creating a Lambda function that will automatically clean up old files in an S3 bucket.

### Task: Automate the deletion of files older than 30 days in a specific S3 bucket.
#### Step 1: S3 Setup
1. Navigate to the S3 dashboard and create a new bucket. <img width="1994" height="1128" alt="image" src="https://github.com/user-attachments/assets/e17f7c66-2867-4c4c-b684-94e7db9eac64" />
2. Upload multiple files to this bucket, ensuring that some files are older than 30 days (you may need to adjust your system's date temporarily for this or use old files). <img width="1970" height="1462" alt="image" src="https://github.com/user-attachments/assets/081bdd2e-11fb-4bc4-8b19-c0e8f8f2e583" /> <img width="1988" height="1456" alt="image" src="https://github.com/user-attachments/assets/6fda539e-adbc-4386-ae85-e200ed4c3a91" />


#### Step 2: Lambda IAM Role
1. In the IAM dashboard, create a new role for Lambda.
