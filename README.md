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

Create a job submittal file by running the following command "cat >> hw.job << EOF"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/cat%20%3E%3E%20hw.job%20%3C%3C%20EOF_1.png)

Run the following command "qsub hw.job"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Job_1.png)

Run the following command "cat hello_all.out"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/hello_all.out.png)

Run the following command "cat helloworld.o1"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/helloworld.o1.png)

log out of master instance by typing exit then press enter.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/exit.png)

Create an Amazon EBS Volume Snapshot for cluster reusability by going to the master instace in the EC2 console. click on the instance and click on the description tab. Scroll down to find block devices and click on /dev/sdb.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/dev:sdb.png)

Click on the ESB ID (volume ID)

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Block_Device.png)

From the action dropdown list select create snapshot.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Create_Snapshot_1.png)

Add a description of the snapshot and create a tag. Then click create snapshot.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Tag_1.png)

Save the snapshot ID in a text file.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/snapshotIDtextfile.png)

Edit the config file after the line that says maintain_initial_size = true with the following "ebs_settings = helloebs"

Add this to the end using the EBS snapshot ID from before:

[ebs helloebs]

ebs_snapshot_id = YOUR-SNAPSHOT-ID

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/ebs_shapshot_id.png)

This allows you to be able to launch a new cluster and the previously created volume and software is automatically available on the shared drive. For example, you can create a new cluster called parallelcluster-HelloCluster2.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/ParallelCluster2.png)

Go to template and then click on View in Designer.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/template_view_In_designer.png)

You can see a visual representation of the Cluster.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Desiner_rep.png)

Click the file symbol, save the template locally, and give it a descriptive name.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Tempate_Save.png)

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Template_Name.png)

To delete and clean up the cluster, check the list of clusters by running the following CLI command "pcluster list"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/pcluster_list.png)

Delete the cluster by entering the following CLI command "pcluster delete YOUR-CLUSTER-NAME"

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/pcluster_delete.png)

Navigate to the EC2 console and select snapshots under EBS. Select the actions dropdown and click delete. It will ask for confirmation. Click yes, delete.

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/EBS_Actions.png)

![alt text](https://github.com/doyle199/AWS_Deploying-an-Elastic-HPC-Cluster/blob/master/Delete_Snapshot.png)
