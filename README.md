# Creating an AWS Cluster

This repository provides some `ansible` playbooks to create a micro cluster in
AWS. The `ansible` `ec2` cloud modules use `boto` and `python`
[bindings](https://aws.amazon.com/developers/getting-started/) to allow you
to create and manage `ec2` instances and `vpc` services remotely.

This isn't a substitues for the
[docs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html), just
a helping hand to automate the steps

The following documents my experience with AWS and Ansible. I'm also using it
to experiment with AWS features. Follow these steps to get and `ec2` cluster of
your own up and running.

## Prerequisites

* Get an AWS account and get on the [free tier](https://aws.amazon.com/free/)
* Review the AWS Identity and Access Management (IAM) [docs](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)
* Create an API [Access Key](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey) so `ansible` can communicate with AWS
* Create and SSH and store it in `~/.ssh/id_rsa.pub`, this will be imported into the instances so that you can access them via `ssh`
* Store the access keys in `~/.aws/credentials`

```
>> cat ~/.aws/credentials

[default]
aws_access_key_id=ABCDEFGHIJKLMNOPQRST
aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXX+YYYYYYYYYYYYYYYYYYY
```
* **Optional -** Install AWS CLI tool, this can be useful for debugging
    * `sudo pip install awscli --user` (on Centos 7)
    * or go to the CLI [ docs](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)
* **Optional -** Configure AWS default region and output for the CLI tool

```
>> cat ~/.aws/config

[default]
region=eu-west-1
output=text
```

## Playbooks

The playbooks setup the simple cluster defined in
`aws-cluster/group_vars/all/vars.yml`.

There are three main playbooks;
1. `aws-cluster/plays/create_cluster.yml`
2. `aws-cluster/plays/configure_cluster.yml`
3. `aws-cluster/plays/terminate_cluster.yml`

The `create_cluster.yml` will setup 6 `ec2` instances a virtual private cloud
`vpc` subnets and a security group that will allow traffic on `ssh` and `icmp`
ports for connecting and pinging. It also exposes the micro cluster to the
internet via an internet gateway `igw` and by configuring the default route
table create by the `vpc`. It all so imports your `~/.ssh/id_rsa.pub` key to
AWS and includes it in all of the instances so that you can `ssh` to them.

The `configure_cluster.yml` configures `ntp` on the nodes and runs a shell
command.

The `terminate_cluster.yml` tears down all of the infrastructure defined in
`aws-cluster/group_vars/all/vars.yml`.

## Usage

```
# Create the cluster

>> ansible-playbook plays/create_cluster.yml

# Check AWS GUI or use AWS CLI to get the public hostnames of instances

>> ping ec2-52-211-222-169.eu-west-1.compute.amazonaws.com
>> ssh  centos@ec2-52-211-222-169.eu-west-1.compute.amazonaws.com

# Run some configuration on the cluster using the dynamic hosts file

>> ansible-playbook plays/configure_cluster.yml

# Terminate the cluster

>> ansible-playbook plays/terminate_cluster.yml
```
