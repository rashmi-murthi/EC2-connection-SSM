
## Attach IAM Role to EC2:
1. **Create IAM Role**
   - Use `AmazonSSMManagedInstanceCore` managed policy.
   - This allows the instance to connect to Systems Manager.  

   ![Create IAM role](images/iam-role.png)

2. **Add CloudWatch Logs Policy** (for session logging)
   - Create a custom inline policy (example name: `CustomCloudWatchLogsPolicy`):

   <details>
   <summary>Click to expand JSON</summary>

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:DescribeLogGroups",
                   "logs:DescribeLogStreams",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents"
               ],
               "Resource": "*"
           }
       ]
   }
</details>

![Attach policies to role](images/policy-attach.png)

3. **Attach this role to the EC2**

![Attach role to EC2 instance](images/iamrole-ec2.png)

## Attach KMS Key Policy (Optional – For Encrypted Session Logs)

### When creating a KMS key, add the following policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowSystemsManagerUse",
            "Effect": "Allow",
            "Principal": {
                "Service": "ssm.amazonaws.com"
            },
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:ReEncrypt*",
                "kms:GenerateDataKey*",
                "kms:DescribeKey"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowEC2InstanceRoleUse",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::YOUR_ACCOUNT_ID:role/EC2SSMRole"
            },
            "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": "*"
        }
    ]
}
```
