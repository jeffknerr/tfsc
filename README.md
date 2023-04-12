# tfsc
terraform source code

Trying out the terraform tutorials...



```
 9995  4/12/2023 08:10  mutt -f mdir/irilyth
 9998  4/12/2023 09:08  sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
 9999  4/12/2023 09:08  wget -O- https://apt.releases.hashicorp.com/gpg | \\ngpg --dearmor | \\nsudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg\n
10001  4/12/2023 09:09  tmux
10004  4/12/2023 09:09  ls /usr/share/keyrings
10005  4/12/2023 09:10  gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint
10006  4/12/2023 09:12  echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \\nhttps://apt.releases.hashicorp.com $(lsb_release -cs) main" | \\nsudo tee /etc/apt/sources.list.d/hashicorp.list\n
10007  4/12/2023 09:12  ls -al /etc/apt/sources.list.d
10008  4/12/2023 09:12  sudo apt update
10009  4/12/2023 09:12  sudo apt-get  install terraform
10010  4/12/2023 09:13  terraform -help
10011  4/12/2023 09:13  terraform -help plan
10012  4/12/2023 09:13  terraform -install-autocomplete
10013  4/12/2023 09:14  mkdir learn-terraform-docker-container
10014  4/12/2023 09:14  cd learn-terraform-docker-container
10023  4/12/2023 09:17  docker ps
10024  4/12/2023 09:18  docker images
10030  4/12/2023 09:21  cd ~/.aws
10032  4/12/2023 09:22  cat credentials
10034  4/12/2023 09:23  mkdir learn-terraform-aws-instance
10039  4/12/2023 09:28  which aws
10040  4/12/2023 09:29  cd /local
10041  4/12/2023 09:29  ll aws
10042  4/12/2023 09:30  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
10043  4/12/2023 09:30  ll aws*
10044  4/12/2023 09:31  unzip awscliv2.zip
10045  4/12/2023 09:31  cd aws
10047  4/12/2023 09:31  more install
10048  4/12/2023 09:32  sudo ./install
10049  4/12/2023 09:32  aws --version

10059  4/12/2023 09:47  vim main.tf
10066  4/12/2023 09:52  terraform init
10069  4/12/2023 09:52  ls -al .terraform
10070  4/12/2023 09:52  ls -al .terraform/providers
10071  4/12/2023 09:52  ls -al .terraform/providers/registry.terraform.io
10072  4/12/2023 09:52  ls -alR .terraform/providers/registry.terraform.io
10073  4/12/2023 09:53  file  .terraform/providers/registry.terraform.io/hashicorp/aws/4.62.0/linux_amd64/terraform-provider-aws_v4.62.0_x5
10076  4/12/2023 09:54  cat main.tf
10080  4/12/2023 09:57  ls -al
10081  4/12/2023 09:57  more terraform.tfstate

10088  4/12/2023 10:26  terraform state list
10093  4/12/2023 10:33  terraform destroy
10094  4/12/2023 10:34  vim main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region = "us-east-2"
}

resource "aws_instance" "app_server" {
  ami                    = "ami-0f35413f664528e13"
  instance_type          = "t2.micro"
  key_name               = "jeffknerr"
  vpc_security_group_ids = [aws_security_group.main.id]
  tags = {
    Name = "JeffExampleAppServerInstance"
  }
}

resource "aws_security_group" "main" {
  egress = [
    {
      cidr_blocks      = ["0.0.0.0/0", ]
      description      = ""
      from_port        = 0
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "-1"
      security_groups  = []
      self             = false
      to_port          = 0
    }
  ]
  ingress = [
    {
      cidr_blocks      = ["0.0.0.0/0", ]
      description      = ""
      from_port        = 22
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 22
    }
  ]
}
10095  4/12/2023 10:37  terraform fmt
10096  4/12/2023 10:37  terraform validate
10097  4/12/2023 10:37  terraform plan
10098  4/12/2023 10:38  terraform apply
10101  4/12/2023 10:43  ssh -i "~/.ssh/jeffknerr.pem" admin@ec2-18-218-81-220.us-east-2.compute.amazonaws.com
```


When using Terraform in production, we recommend that you use a version control
system to manage your configuration files, and store your state in a remote
backend such as Terraform Cloud or Terraform Enterprise.


```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region = "us-east-2"
}

resource "aws_instance" "app_server" {
  ami                    = "ami-0f35413f664528e13"
  instance_type          = "t2.micro"
  key_name               = "jeffknerr"
  vpc_security_group_ids = [aws_security_group.main.id]
  tags = {
    Name = "JeffExampleAppServerInstance"
  }
}

resource "aws_security_group" "main" {
  egress = [
    {
      cidr_blocks      = ["0.0.0.0/0", ]
      description      = ""
      from_port        = 0
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "-1"
      security_groups  = []
      self             = false
      to_port          = 0
    }
  ]
  ingress = [
    {
      cidr_blocks      = ["130.58.0.0/16", ]
      description      = "in ssh"
      from_port        = 22
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 22
    },
    {
      cidr_blocks      = ["130.58.0.0/16", ]
      description      = "in http"
      from_port        = 80
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 80
    },
    {
      cidr_blocks      = ["130.58.0.0/16", ]
      description      = "in https"
      from_port        = 443
      ipv6_cidr_blocks = []
      prefix_list_ids  = []
      protocol         = "tcp"
      security_groups  = []
      self             = false
      to_port          = 443
    }
  ]
}
```


