# AWS-Deploying-an-Elastic-HPC-Cluster

Create an AWS EC2 pair and install AWS CLI version2

![alt text](https://github.com/doyle199/Deploying-an-Elastic-HPC-Cluster/blob/master/AWS_CLI.png?raw=true)

Install Python and Pip3 (using Homebrew for Mac for this tutorial)
Install AWS ParallelCluster with the following command "sudo pip3 install --upgrade was-parallelcluster"

![alt text](https://github.com/doyle199/Deploying-an-Elastic-HPC-Cluster/blob/master/Install_pip3.png?raw=true)

Create and Admin IAM and setup the IAM credentials in CLI with the following command "aws configure". Enter the AWS Access Key ID, AWS Secret Access Key, Default region name, Default output format.

Test the IAM credentials in CLI with the folllowing command "aws s3 ls"

![alt text](https://github.com/doyle199/Deploying-an-Elastic-HPC-Cluster/blob/master/aws_s3_ls.png?raw=true)

Configure and launch ParallelCluster with the following command "pcluster configure"

Choose a region, enter key pair name, enter allowed values for scheduler, enter allowed values for operating system, enter the maximum cluster size (instances), Enter the master instance type. 

