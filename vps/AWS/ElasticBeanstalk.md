
## EB

aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $FILE --source-bundle


aws elasticbeanstalk create-application-version --application-name application-name --version-label voting-back_app --source-bundle

Install [EB Cli](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install-advanced.html) in the
server:
```sh
# Install PIP and Cli
sudo dpkg --configure -a
sudo apt install python3-pip
pip install awsebcli --upgrade --user
source ~/.profile
```

Sugin [Docker with EB](https://docs.amazonaws.cn/en_us/elasticbeanstalk/latest/dg/docker-multicontainer-migration.html#docker-multicontainer-migration.procedure),


https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/docker.html

```sh
eb init -p docker application-name
eb create env-application-name
eb deploy env-application-name --verbose
eb open  env-application-name
eb status env-application-name
eb terminate environment-name
eb events -f

eb local run --port 3100

aws ecr create-repository --repository-name application-name
```

Configure the [EB environment variables](https://aws.amazon.com/premiumsupport/knowledge-center/elastic-beanstalk-pass-variables/):

```yml
branch-defaults:
  45-docker-production:
    environment: env-application-name
    group_suffix: null
environment-defaults:
  env-application-name:
    branch: null
    repository: null
global:
  application_name: application-name
  branch: null
  default_ec2_keyname: null
  default_platform: Docker
  default_region: us-west-2
  include_git_submodules: true
  instance_profile: null
  platform_name: null
  platform_version: null
  profile: null
  repository: null
  sc: git
  workspace_type: Application
```


## IAM Role

Create an [IAM Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html)
that will hold CF , Instance and [S3](https://aws.amazon.com/premiumsupport/knowledge-center/s3-access-denied-listobjects-sync/) permissions:
* AmazonS3FullAccess (s3:GetObject, s3:ListObjectsV2, s3:ListBucket)
* CloudFrontFullAccess (cloudfront:CreateInvalidation)
* AmazonSSMManagedInstanceCore


The change our EC2 instance to use this new IAM Role.