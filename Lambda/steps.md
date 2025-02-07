### **Steps to Create the AWS Lambda Function for Deleting Stale EBS Snapshots:**

#### **Step 1: Set Up AWS Lambda Function**
1. **Create a Lambda Function in AWS Console:**
   - Go to the AWS Management Console.
   - Navigate to **Lambda** service and click on **Create Function**.
   - Select **Author from Scratch**.
   - Choose **Python** as the runtime (e.g., Python 3.8 or later).
   - Name the function (e.g., **DeleteStaleEBSSnapshots**).
   - Set **Execution role** to allow Lambda to access EC2 and describe snapshots.

#### **Step 2: Create IAM Role and Permissions**
2. **Create an IAM Role for Lambda:**
   - Create an IAM role with permissions to interact with EC2 and EBS snapshots.
   - Attach the following policies to the IAM role:
     - `AmazonEC2ReadOnlyAccess` (for fetching EC2 details and snapshots).
     - `AmazonEC2FullAccess` (for deleting snapshots).
     - `AWSLambdaBasicExecutionRole` (for logging to CloudWatch).
   - Attach this IAM role to your Lambda function.

#### **Step 3: Write Python Code for Lambda Function**
3. **Write the Lambda Function Code:**
   - The function should:
     - **Fetch all snapshots** using `describe_snapshots()`.
     - **Retrieve active EC2 instances** using `describe_instances()`.
     - **Check if a snapshot is stale** by verifying whether its associated volume is attached to any active or stopped instance.
     - **Check the snapshot age** and delete snapshots older than 6 months (or a custom time) that are no longer in use.
   
   Example code:

```python
import boto3
from datetime import datetime, timedelta

# Initialize EC2 client
ec2 = boto3.client('ec2')

# Lambda handler function
def lambda_handler(event, context):
    # Get all snapshots
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']
    
    # Get all active EC2 instances
    instances = ec2.describe_instances(
        Filters=[{'Name': 'instance-state-name', 'Values': ['running', 'stopped']}]
    )
    
    # Get volumes attached to EC2 instances
    volumes_in_use = set()
    for reservation in instances['Reservations']:
        for instance in reservation['Instances']:
            for volume in instance['BlockDeviceMappings']:
                volumes_in_use.add(volume['Ebs']['VolumeId'])

    # Current date for checking the snapshot age
    current_date = datetime.now()

    # Loop through each snapshot to check if it's stale
    for snapshot in snapshots:
        snapshot_id = snapshot['SnapshotId']
        snapshot_creation_date = snapshot['StartTime'].replace(tzinfo=None)

        # Calculate snapshot age (older than 6 months is stale)
        if (current_date - snapshot_creation_date) > timedelta(days=180):
            # Check if the snapshot's volume is in use
            volume_id = snapshot['VolumeId']
            if volume_id not in volumes_in_use:
                print(f"Deleting stale snapshot: {snapshot_id}")
                ec2.delete_snapshot(SnapshotId=snapshot_id)

    return {
        'statusCode': 200,
        'body': 'Stale snapshots checked and deleted successfully.'
    }
```

#### **Step 4: Set Up Triggers for Lambda Function**
4. **Configure Event Source:**
   - You can trigger this Lambda function on a **schedule** using Amazon CloudWatch Events (cron jobs).
   - Create a **CloudWatch Rule** to run the Lambda function on a schedule (e.g., every day or once a week).

   Example cron expression to run the Lambda function every day:
   ```bash
   cron(0 0 * * ? *)
   ```

#### **Step 5: Test the Lambda Function**
5. **Test the Lambda Function:**
   - Create a test event to simulate the function execution. You can start by manually triggering the Lambda to ensure it's properly fetching snapshots and deleting stale ones.
   - Check the logs in **CloudWatch Logs** to verify the actions performed by the Lambda function.

#### **Step 6: Monitor and Optimize Lambda Performance**
6. **Monitor Lambda Execution:**
   - Check CloudWatch metrics and logs to monitor the Lambda’s performance.
   - Track **function execution time**, **errors**, and any **deletions** made by the function.

   Adjust the function’s timeout and memory settings based on your needs.

#### **Step 7: Fine-tune and Deploy**
7. **Fine-tune the Function:**
   - If required, adjust the snapshot age criteria (e.g., change the 6-month rule).
   - Add logging or notifications (using SNS or CloudWatch) to alert administrators when snapshots are deleted.
  
8. **Deploy to Production:**
   - After thorough testing, deploy the Lambda function to production and ensure it runs as expected.

---

### **Outcome:**
- **Automatic Snapshot Deletion:** This Lambda function will help automate the process of finding and deleting **stale EBS snapshots** that are no longer associated with any EC2 instance, which will reduce unnecessary storage costs.
- **Efficient Resource Management:** It ensures that resources are used efficiently and that your AWS bill is optimized.

This project demonstrates a **real-world use case** of AWS Lambda for **automating cloud resource cleanup** and **cost optimization**, which is highly relevant for DevOps roles.