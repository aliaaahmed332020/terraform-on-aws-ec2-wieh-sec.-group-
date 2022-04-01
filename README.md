# terraform-on-aws-ec2-wieh-sec.-group-
first i installed terraform and awscli
then i wrote the terraform file as following:
provider "aws" {
    profile = "default"
    region = "eu-west-3"
}

resource "aws_default_vpc" "main" {

  tags = {
    Name = "main"
  }
}
resource "aws_security_group" "aliaa-sec-group" {
  name        = "aliaa-sec-group"
  description = "Allow tls inbound traffic"
  vpc_id      = aws_default_vpc.main.id

  ingress {
    description      = "inbound rules from VPC"
    from_port        = 443
    to_port          = 443
    protocol         = "tcp"
    cidr_blocks      = [aws_default_vpc.main.cidr_block]


  }
  ingress {
    description      = "inbound rules from VPC"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = [aws_default_vpc.main.cidr_block]

  }
  ingress {
    description      = "inbound rules from VPC"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = [aws_default_vpc.main.cidr_block]

  }
  egress {
    from_port        = 0
    to_port          = 0
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }

  tags = {
    Name = "aws_security_group"
    instance_name = "aws_instance"
  }
}

resource "aws_instance" "ec2-instance" {
    ami = "ami-0960de83329d12f2f"
    instance_type = "t2.micro"
    security_groups = [aws_security_group.aliaa-sec-group.name]
    key_name = "Aliaa-key-pair"
}
