## Deploying Amazon Aurora MYSQL Database in AWS

### Resources used in this project:
-   Amazon VPC
-   Public & Private Subnets
-   Internet Gateway
-   Public & Private Route Tables
-   Security groups and NACL
-   EC2 Instance
-   Amazon Aurora Database


### Project Architecture:
###### Deploying this Quick Start for a new virtual private cloud (VPC) with default parameters builds the following Aurora MySQL environment in the AWS Cloud.
![Project Diagram](https://github.com/ahsan598/aws-wordpress-website/blob/main/aws-wordpress-website-diagram.svg)


### Deployment:
As shown in Figure, sets up the following:

-A Virtual Private Cloud (VPC) configured with public and private subnets, to provide with our own virtual network on AWS.

-Your VPC must have two private subnets in different Availability Zones for the database instances.

###### In the public subnets:

-Managed network address translation (NAT) gateways to allow outbound internet access for resources in the private subnets.

-A Linux bastion host in an Auto Scaling group to allow inbound Secure Shell (SSH) access to resources in the private subnets.

-In the private subnets, an Aurora MySQL database (DB) cluster in a security group, including one DB reader and one DB writer.

-An Amazon CloudWatch alarm to monitor the CPU on the bastion host and send alarm notifications using Amazon Simple Notification Service (Amazon SNS).

-An encryption key using AWS Key Management Service (AWS KMS). The key enables encryption at rest for the Aurora DB cluster.


### Test Deployment:

-To test the deployment, confirm that the MySQL database is accepting connections by following below steps:

1. Download the latest version of MySQL Workbench, and install it on the workstation from which you will be connecting to the Aurora MySQL DB cluster.
2. From the AWS CloudFormation console, on the BastionStack Outputs tab, note the value for EIP1.
3. From the AWS CloudFormation console, on the AuroraStack Outputs tab, note the values of DBName, DBMasterUsername, AuroraClusterEndpoint, and AuroraClusterPort
4. Create an SSH tunnel to the bastion host using the following command,
"ssh -N -L <AuroraClusterPort>:<AuroraClusterEndpoint>:<AuroraClusterPort> ec2-user@EIP1 -i <KeyPairName>"

A message appears indicating that you’ve connected to the bastion host.

5. Launch MySQL Workbench on your workstation.
6. On the Database menu, choose Connect to Database.
7. Enter the following in the Connect to Database dialog box.
    a. In the Hostname field, enter 127.0.0.1
    b. In Port field, enter the value for the AuroraClusterPort parameter.
    c. In Username, enter the value for the DBMasterUsername parameter.
    d. Choose OK.

8. In the Connect to MySQL Server dialog box, enter the administrator password (DBMasterUserPassword) that you entered during stack creation.
9. A MySQL Workbench dashboard appears.
10. In the Navigator pane, under PERFORMANCE, choose Dashboard. Database-performance metrics appears.
11. Terminate the SSH tunnel by pressing Ctrl+C. You’ve completed the testing.



##### Learned from coursera