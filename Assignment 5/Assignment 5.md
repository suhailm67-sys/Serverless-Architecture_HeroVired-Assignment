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
1. Navigate to the Lambda dashboard, create a new function by choosing Python 3.14 as the runtime and assign the custom role created in the previous step.
