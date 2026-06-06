# Serverless-Architecture_HeroVired-Assignment
HeroVired Assignment on Serverless Architecture using AWS Lambda and Boto 3

## Assignment 1: Automated Instance Management Using AWS Lambda and Boto3
Objective: In this assignment, we will gain hands-on experience with AWS Lambda and Boto3, Amazon's SDK for Python. We will create a Lambda function that will automatically manage EC2 instances based on their tags.

### Tasks: You're tasked to automate the stopping and starting of EC2 instances based on tags.
#### Step 1: Create two EC2 instances
1. Navigate to the EC2 dashboard and create two new t2.micro instances (or any other available free-tier type)
   1. Create EC2 instance and give a tag name to instance - Actions → Instance Settings → Manage Tags
   2. Tag the first instance with a key `Action` and value `Auto-Stop`
   3. Tag the second instance with a key `Action` and value `Auto-Start`.
<img width="1581" height="258" alt="image" src="https://github.com/user-attachments/assets/0a7927ce-d232-4a2c-ae61-16dff6b95aea" />
