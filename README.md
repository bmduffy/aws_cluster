# Creating an AWS Cluster

This is a guide on how to create some basic clusters in a AWS. My goal is to
help people get up and running with AWS without having to wade through all of
the AWS docs.

There are many different API [bindings](https://aws.amazon.com/developers/getting-started/)
for AWS including Python. But to make things super simple, I decided to use the
the Ansible AWS modules to interact with the API. These use Python and Boto
and provide a nice declarative method of interacting with AWS.

You will be leveraging the Amazon Elastic Compute Cloud (EC2) to create
clusters from `ec2` instances. You should also review the documentiation
[here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) when
you're up and running.

The following documents my experience with AWS and Ansible. I'm also using it
to experiment with AWS features. Follow these steps to get and `ec2` cluster of
your own up and running.

## 0. Get An AWS Account

Get an AWS account and get on the [free tier](https://aws.amazon.com/free/).

## 1. Get AWS Secruity Credentials

There is a lot of useful information in the AWS security credentials
[docs](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html),
which you should review at some point. To get up and running with Ansible and
AWS you will need;

* [Access Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey) to make programmatic calls to AWS API actions.
* [Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)
  to access EC2 instances via SSH

Store these keys somewhere outside your repository where you can encrypt them
```
>> tree ~/.aws/

/home/brian.duffy/.aws/
|-- config
`-- credentials

>> cat ~/.aws/config

[default]
region=eu-west-1
output=text

[profile user1]
region=eu-west-1
output=text

>> cat ~/.aws/credentials

[default]
aws_access_key_id=ABCDEFGHIJKLMNOPQRST
aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXX+YYYYYYYYYYYYYYYYYYY

[user1]
aws_access_key_id=TSRQPONMLKJIHGFEDCBA
aws_secret_access_key=YYYYYYYYYYYYYYYYYYYYY+XXXXXXXXXXXXXXXXXXX

```

## 2. Install AWS CLI Client

## Some Useful Commands
