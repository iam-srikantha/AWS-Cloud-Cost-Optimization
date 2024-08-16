# AWS-Cloud-Cost-Optimization
## Identifying Stale EBS Snapshots
In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.

## Description
The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.
### Example
A person creates an EC2 instance and takes a snapshot every day for backup purposes. Since snapshots are chargeable by AWS, we don't need to keep the old ones. We can delete them to optimize costs.

## Implementation
### Creating EC2 and Snapshot
- Go to AWS Console
- Navigate the EC2
- Create the EC2 Instance
- Create the Snapshot

### Creating Lambda Function
- Go to AWS Console
- Navigate Lambda Function
- Create Function
  - Author from scratch => Function name(cost-optimize-ebs-snpshot) => Runtime (Python 3.12) => Create Function
  - Go to github Account @iam-srikantha Navigate the AWS-Cloud-Cost-Optimization/ebs_stale_snapshots.py > Copy the code
  - Go back created Lambda function > scrolldown > Code > lambda function - Replace that Code > Save this code using Cntrl+S > Deploy > then Test Project > provide the event name - test(Because we mannually triggering the Event) > Save
- Status- Failed Because bydefault the lambda function execution is only 3 seconds and it is also failing some permission also.
 ![image](https://github.com/user-attachments/assets/378e60c5-482e-4396-ab8b-8f67292c29ee)

- Go to Configuration > Edit > Timeout change the time 10 seconds - Save
- Provide Permission
   Configuration > Permission > click cost-optimize-ebs-snpshot-role link > Add permission > Attach policies > attach existing policies (How to creating the policies refer to below lines) > after attaching the policies - Add permission.
     1. create policies > IAM > Policies > create policy > Service - EC2 > Filter Action search - Write(Delete Snapshot), List(Describe snapshot), Resources(All) > Next > Policy Name (cost-optimization-ebs) > create policy
     2. create policies > IAM > Policies > create policy > Service - EC2 > Filter Action search - List(Describevolume), (Describeinstance) > Next > Policy Name (ec2-permission > create policy)

#### Note: Now Terminate the EC2 instance that instance will delete the volume as well but snapshot cannot be deleted

