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
2. Lambda IAM Role
   1. In the IAM dashboard, create a new role for Lambda
   2. Attach the `AmazonEC2FullAccess` policy to this role <img width="1552" height="681" alt="image" src="https://github.com/user-attachments/assets/46d7767a-c095-4c25-b07b-95bb0113dd94" />
3. Lambda Function Creation
   1. Navigate to the Lambda dashboard and create a new function
   2. Choose Python 3.x as the runtime
   3. Assign the IAM role created in the previous step <img width="1875" height="522" alt="image" src="https://github.com/user-attachments/assets/063b1c1f-902a-447b-9997-e2678573544a" />
   4. Write the Boto3 Python script to:
      1. Initialize a boto3 EC2 client
      2. Describe instances with `Auto-Stop` and `Auto-Start` tags
      3. Stop the `Auto-Stop` instances and start the `Auto-Start` instances
      4. Print instance IDs that were affected for logging purposes.
         ```import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):

    auto_stop_instances = []
    auto_start_instances = []

    # Find instances tagged Auto-Stop
    stop_response = ec2.describe_instances(
        Filters=[
            {
                'Name': 'tag:Action',
                'Values': ['Auto-Stop']
            }
        ]
    )

    # Extract instance IDs
    for reservation in stop_response['Reservations']:
        for instance in reservation['Instances']:
            auto_stop_instances.append(instance['InstanceId'])

    # Find instances tagged Auto-Start
    start_response = ec2.describe_instances(
        Filters=[
            {
                'Name': 'tag:Action',
                'Values': ['Auto-Start']
            }
        ]
    )

    # Extract instance IDs
    for reservation in start_response['Reservations']:
        for instance in reservation['Instances']:
            auto_start_instances.append(instance['InstanceId'])

    # Stop instances
    if auto_stop_instances:
        ec2.stop_instances(
            InstanceIds=auto_stop_instances
        )

        print(
            f"Stopped Instances: {auto_stop_instances}"
        )

    # Start instances
    if auto_start_instances:
        ec2.start_instances(
            InstanceIds=auto_start_instances
        )

        print(
            f"Started Instances: {auto_start_instances}"
        )

    return {
        'statusCode': 200,
        'body': 'EC2 automation completed'
    }```
<img width="1388" height="612" alt="image" src="https://github.com/user-attachments/assets/c18ebf01-7d25-4110-aa7e-368d92b062ba" />
<img width="1778" height="252" alt="image" src="https://github.com/user-attachments/assets/0f219022-2b28-46b8-b080-d3a049eb722f" />
