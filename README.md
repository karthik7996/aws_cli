# aws cli installation
1. curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
2. unzip awscliv2.zip
3. sudo ./aws/install
--------------------------------------------------------------------------------
# aws_cli
1. aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
2. chmod 400 MyKeyPair.pem
3. aws ec2 delete-key-pair --key-name MyKeyPair

-----------------------------------------------------------------------------------------------
# Creating Key-Pair:
1. aws ec2 create-key-pair --key-name Sharath --query 'KeyMaterial' --output text > Sharath.pem

# Creating Security Group:
1. aws ec2 create-security-group --group-name my-sg --description "My security group"
2. aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 22 --cidr 0.0.0.0/0
3. aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 80 --cidr 0.0.0.0/0
4. aws ec2 describe-security-groups --group-names my-sg
----------------------------------------------------------------------------------------------------
# Creating/Deleting EC2-Instance:
1. aws ec2 run-instances --image-id ami-0a8b4cd432b1c3063 --count 1 --instance-type t2.micro --key-name Sharath --security-group-ids sg-08f7061d2068b6df7
2. aws ec2 create-tags --resources i-05ae4a2d82fcc2c8d --tags Key=Name,Value=Sharath
3. aws ec2 terminate-instances --instance-ids i-05ae4a2d82fcc2c8d
-------------------------------------------------------------------------------------------------------
# Creating/Deleting Load-Balance:
1. aws elb create-load-balancer --load-balancer-name sharath-load-balancer --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" --availability-zones us-east-1a us-east-1b --security-groups sg-08f7061d2068b6df7
2. aws elb register-instances-with-load-balancer --load-balancer-name sharath-load-balancer --instances i-00f771807b71e967a
3. aws elb delete-load-balancer --load-balancer-name sharath-load-balancer
-----------------------------------------------------------------------------------------------------------
# Creating/Deleting Amazon S3:
1. aws s3 mb s3://bucket-name
2. aws s3 rb s3://bucket-name
3. Copying: aws s3 cp filename s3://bucket-name
--------------------------------------------------------------------------------------------------------------
# Creating VPC:
1. aws ec2 create-vpc --cidr-block 192.0.0.0/16 --query Vpc.VpcId --output text
2. aws ec2 create-subnet --vpc-id vpc-026695d0114ad1eed --cidr-block 192.0.1.0/24
3. aws ec2 create-subnet --vpc-id vpc-026695d0114ad1eed --cidr-block 192.0.2.0/24
4. aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text
5. aws ec2 attach-internet-gateway --vpc-id vpc-026695d0114ad1eed --internet-gateway-id igw-0eb1bc19394187004
6. aws ec2 create-route-table --vpc-id vpc-026695d0114ad1eed --query RouteTable.RouteTableId --output text
7. aws ec2 create-route --route-table-id rtb-0888cd56899632bd0 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-0eb1bc19394187004
8. aws ec2 describe-route-tables --route-table-id rtb-0888cd56899632bd0
9. aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-026695d0114ad1eed" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"
10. aws ec2 associate-route-table  --subnet-id subnet-0c1c9f6c96182489f --route-table-id rtb-0888cd56899632bd0
---------------------------------------------------------------------------------------------------------------------------
1. aws ec2 create-vpc --cidr-block 10.0.0.0/16 --query Vpc.VpcId --output text
2. aws ec2 create-subnet --vpc-id vpc-07e4ae66175b7f4e1 --cidr-block 10.0.1.0/24
3. aws ec2 create-subnet --vpc-id vpc-07e4ae66175b7f4e1 --cidr-block 10.0.2.0/24
4. aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text
5. aws ec2 attach-internet-gateway --vpc-id vpc-07e4ae66175b7f4e1 --internet-gateway-id igw-00ee7569aeec0c2ff
6. aws ec2 create-route-table --vpc-id vpc-07e4ae66175b7f4e1 --query RouteTable.RouteTableId --output text
7. aws ec2 create-route --route-table-id rtb-05fc949bb4e81000f --destination-cidr-block 0.0.0.0/0 --gateway-id igw-00ee7569aeec0c2ff
8. aws ec2 describe-route-tables --route-table-id rtb-05fc949bb4e81000f
9. aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-07e4ae66175b7f4e1" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"
10. aws ec2 associate-route-table  --subnet-id subnet-004a8fbe5ec6be5c8 --route-table-id rtb-05fc949bb4e81000f
11. aws ec2 associate-route-table  --subnet-id subnet-0493b62c84bf851bd --route-table-id rtb-05fc949bb4e81000f
-------------------------------------------------------------------------------------------------------------------------------
# Creating VPC peering:
1. aws ec2 create-vpc-peering-connection --vpc-id vpc-07e4ae66175b7f4e1 --peer-vpc-id vpc-026695d0114ad1eed

Deleting Vpc:
1. aws ec2 delete-subnet --subnet-id subnet-0c1c9f6c96182489f
2. aws ec2 delete-route-table --route-table-id rtb-0888cd56899632bd0
3. aws ec2 detach-internet-gateway --internet-gateway-id igw-0eb1bc19394187004 --vpc-id vpc-026695d0114ad1eed
4. aws ec2 delete-internet-gateway --internet-gateway-id igw-0eb1bc19394187004
5. aws ec2 delete-vpc --vpc-id vpc-026695d0114ad1eed


CloudFormation Script:
{ "AWSTemplateFormatVersion" : "2010-09-09",
  "Resources" : {
    "EC2Instance" : {
      "Type" : "AWS::EC2::Instance",
      "Properties" : {
        "ImageId" : "ami-0a8b4cd432b1c3063",
        "KeyName" : "Sharath",
        "InstanceType": "t2.micro"
    }
   }
 }
}

# Creating/Deleting CloudFormation:
1. aws cloudformation create-stack --stack-name myteststack --template-body file://ec2_launching.json
2. aws cloudformation delete-stack --stack-name myteststack
------------------------------------------------------------------------------------------------------------------------
# Creating/Deleting Lambda:
Required Scripts:
exports.handler = function(){
    console.log("sucess fully using cli");
    callback(null,"success");
}

#!/bin/bash
aws lambda create-function \
    --function-name "HelloWorld" \
    --runtime "nodejs6.10" \
    --role "" \
    --handler "app/index.handler" \
    -- timeout 5 \
    --zip-file "fileb://./app.zip" \
    --region "us-east-1"

aws lambda delete-function \
    --function-name my-function
 
# Creating/Deleting IAM:
aws iam create-role --role-name lambda-sharath --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
aws iam delete-role --role-name lambda-sharath
-------------------------------------------------------------------------------------------------------------------------------------
# Creating/Deleting CloudWatch:

# SNS subscribing:
1. aws sns create-topic --name sharath-sns
2. aws sns subscribe --topic-arn arn:aws:sns:us-east-1:602011150591:sharath-sns --protocol email --notification-endpoint my-email-address
3. aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:602011150591:sharath-sns

# Unsubscribing:
1. aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:602011150591:sharath-sns:8a21d249-4329-4871-acc6-7be709c6ea7f
2. aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:602011150591:sharath-sns

# CloudWatch:
1. aws cloudwatch put-metric-alarm --alarm-name cpu-mon --alarm-description "Alarm when CPU exceeds 70 percent" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold  --dimensions "Name=InstanceId,Value=i-0687531b496d219ae" --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:602011150591:sharath-sns --unit Percent
2. aws cloudwatch set-alarm-state --alarm-name sharath-alaram --state-reason "initializing" --state-value OK
3. aws cloudwatch set-alarm-state --alarm-name sharath-alaram --state-reason "initializing" --state-value ALARM

Deleting:
aws cloudwatch delete-metric-stream --name sharath-alaram
-----------------------------------------------------------------------------------------------------------------------------------------------------
12.Creating/Deleting Route53:
