AWSTemplateFormatVersion: "2010-09-09"
Resources:
VPC:
Type: "AWS::EC2::VPC"
Properties:
CidrBlock: "10.0.0.0/16"
Tags:
- Key: "Name"
Value: "MyVPC"
PublicSubnet:
Type: "AWS::EC2::Subnet"
Properties:
CidrBlock: "10.0.1.0/24"
VpcId: !Ref VPC
AvailabilityZone: "us-east-1a"
Tags:
- Key: "Name"
Value: "MyPublicSubnet"
SSMRole:
Type: "AWS::IAM::Role"
Properties:
RoleName: "SSMRole"
AssumeRolePolicyDocument:
Version: "2012-10-17"
Statement:
- Effect: "Allow"
Principal:
Service: "ec2.amazonaws.com"
Action: "sts:AssumeRole"
Policies:
- PolicyName: "SSMPolicy"
PolicyDocument:
Version: "2012-10-17"
Statement:
- Effect: "Allow"
Action:
- "ssm:*"
Resource: "*"
SecurityGroup:
Type: "AWS::EC2::SecurityGroup"
Properties:
GroupDescription: "Allow traffic to ports 80, 443, and 22"
VpcId: !Ref VPC
SecurityGroupIngress:- IpProtocol: "tcp"
FromPort: 80
ToPort: 80
CidrIp: "0.0.0.0/0"
- IpProtocol: "tcp"
FromPort: 443
ToPort: 443
CidrIp: "0.0.0.0/0"
- IpProtocol: "tcp"
FromPort: 22
ToPort: 22
CidrIp: "0.0.0.0/0"
EC2Instance:
Type: "AWS::EC2::Instance"
Properties:
ImageId: "ami-0c94855ba95c71c99" # Amazon Linux 2 AMI
InstanceType: "t2.micro"
KeyName: "my-keypair"
NetworkInterfaces:
- DeviceIndex: "0"
SubnetId: !Ref PublicSubnet
GroupSet:
- !Ref SecurityGroup
AssociatePublicIpAddress: "true"
IamInstanceProfile: !Ref SSMRole
UserData:
Fn::Base64: !Sub |
#!/bin/bash
yum update -y
amazon-linux-extras install -y epel
yum install -y
https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ss
m-agent.rpmAWSTemplateFormatVersion: "2010-09-09"
Resources:
VPC:
Type: "AWS::EC2::VPC"
Properties:
CidrBlock: "10.0.0.0/16"
Tags:
- Key: "Name"
Value: "MyVPC"
PublicSubnet:
Type: "AWS::EC2::Subnet"
Properties:
CidrBlock: "10.0.1.0/24"
VpcId: !Ref VPC
AvailabilityZone: "us-east-1a"
Tags:
- Key: "Name"
Value: "MyPublicSubnet"
SSMRole:
Type: "AWS::IAM::Role"
Properties:
RoleName: "SSMRole"
AssumeRolePolicyDocument:
Version: "2012-10-17"
Statement:
- Effect: "Allow"
Principal:
Service: "ec2.amazonaws.com"
Action: "sts:AssumeRole"
Policies:
- PolicyName: "SSMPolicy"
PolicyDocument:
Version: "2012-10-17"
Statement:
- Effect: "Allow"
Action:
- "ssm:*"
Resource: "*"
SecurityGroup:
Type: "AWS::EC2::SecurityGroup"
Properties:
GroupDescription: "Allow traffic to ports 80, 443, and 22"
VpcId: !Ref VPC
SecurityGroupIngress:- IpProtocol: "tcp"
FromPort: 80
ToPort: 80
CidrIp: "0.0.0.0/0"
- IpProtocol: "tcp"
FromPort: 443
ToPort: 443
CidrIp: "0.0.0.0/0"
- IpProtocol: "tcp"
FromPort: 22
ToPort: 22
CidrIp: "0.0.0.0/0"
EC2Instance:
Type: "AWS::EC2::Instance"
Properties:
ImageId: "ami-0c94855ba95c71c99" # Amazon Linux 2 AMI
InstanceType: "t2.micro"
KeyName: "my-keypair"
NetworkInterfaces:
- DeviceIndex: "0"
SubnetId: !Ref PublicSubnet
GroupSet:
- !Ref SecurityGroup
AssociatePublicIpAddress: "true"
IamInstanceProfile: !Ref SSMRole
UserData:
Fn::Base64: !Sub |
#!/bin/bash
yum update -y
amazon-linux-extras install -y epel
yum install -y
https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ss
m-agent.rpm
