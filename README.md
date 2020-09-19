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

Choose a region(example: us-east-1), enter key pair name(create a key pair in EC2 if one does not have one already), enter allowed values for scheduler(example: slurm), enter allowed values for operating system (example: alinux2), enter the maximum cluster size (instances) (example: 10), Enter the master instance type (example: t2.micro). Automate VPC creation:y, Allowed values for Network Configuration [Master in a public subnet and compute fleet in a private subnet]

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/ParallelCuster_Config.png?raw=true)

Review the new config file with the following command "cat ~/.parallelcluster/config"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/cat%20~:.parallelcluster:config.png?raw=true)

You can edit the config file in a test editor. Change the initial_queue_size = 3, max_queue_size = 3, maintain_initial_size = True, after [cluster default], key_name = YOUR KEY NAME

Launch the AWS ParallelCluster from CLI with the following command "pcluster create HelloCluster"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/CLI_Launch_ParallelCluster_1.png)

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/ParallelCluster-HelloCluster_1.png)

Log in to the master instance with CLI. You can find the master instance in the AWS EC2 console.

Run the command "qhost"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/qhost_1.png)

3 hosts are shown

Next create an mpi_hello_world executable. Run the following command separately: cd /shared, mkdir hw work, cd hw work, cat >> mpi_hello_world.c << EOF

Run the following command: /opt/amazon/openmpi/bin/mpicxx mpi_hello_world.c -o hw.x

An executable file is created.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/mpi_hello_world_1.png)



