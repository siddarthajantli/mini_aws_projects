Cloudwatch Alarma Setup for custom logs
-----------------------------------------------------------------------
Pushing the application logs to cloudwatch to create the alaram in case any anamoly in the system.

**Steps to follow**

1: Create the role with appropriate policy configure to grant access to Cloudwatch.
IAM Policy:

{

  "Version": "2012-10-17",
  
  "Statement": [
  
    {
    
      "Effect": "Allow",
      
      "Action": [
      
        "logs:CreateLogGroup",
        
        "logs:CreateLogStream",
        
        "logs:PutLogEvents",
        
        "logs:DescribeLogStreams"
    
      ],
    
      "Resource": [
        "arn:aws:logs:*:*:*"  
      ]
    
    }
  
  ]

}

2: Configure the IAM role to an instance or bunch of instancess from where we wanted to push the logs [application] to CloudWatch "logs group".
3: Once role is configured / attached. We need to check the status of the awslogsd, if not yet installed then go ahead and install across all instancess.
4: Configure the logs absolute path, region and logs group name into /etc/awslogs/awslogs.conf, as below mentioned example. 

awslogs.conf file contents:

[general]
state_file = /var/lib/awslogs/agent-state
[application_logs]
region = us-east-1
datetime_format = %b %d %H:%M:%S
file = <Application logs absolute Path>
buffer_duration = 5000
log_stream_name = {instance_id} # It will push the logs to instance ID namespace.
initial_position = start_of_file
log_group_name = <Logs Group Name>

5: Make sure regions configured in awscli.conf must match the awslogs.conf.

Note: Installing the logs agent into amazone linux machine. 

yum update -y 
yum install awslogs

Once installation is done we need to start the process. Commands as below.

Start Service : systemctl start awslogsd
Status Check  : systemctl status awslogsd
