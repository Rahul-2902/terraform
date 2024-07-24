# creating vpc with 2 public subnet and 4 private subnet and one private subnet with nginx installed in it using terraform


terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.59.0"
    }
  }
}
provider "aws" {
  region = "us-east-1"
  access_key = "-----------"
  secret_key = "----------"
}

resource "aws_vpc" "vpc" {
  cidr_block       = "198.172.0.0/16"
  instance_tenancy = "default"

  tags = {
    Name = "vpc"
  }
}

resource "aws_subnet" "pub-sub-1" {
  vpc_id     = aws_vpc.vpc.id
  cidr_block = "198.172.1.0/24"

  tags = {
    Name = "pub-sub-1"
  }
}

resource "aws_subnet" "pub-sub-2" {
  vpc_id     = aws_vpc.vpc.id
  cidr_block = "198.172.2.0/24"

  tags = {
    Name = "pub-sub-2"
  }
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.vpc.id

  tags = {
    Name = "igw"
  }
}

resource "aws_route_table" "rt-1" {
    vpc_id = aws_vpc.vpc.id

    route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.igw.id
    }
}

resource "aws_route_table" "rt-2" {
    vpc_id = aws_vpc.vpc.id

    route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.igw.id
    }
}


resource "aws_route_table_association" "asso-1" {
  subnet_id      = aws_subnet.pub-sub-1.id
  route_table_id = aws_route_table.rt-1.id
}

resource "aws_route_table_association" "asso-2" {
  subnet_id      = aws_subnet.pub-sub-2.id
  route_table_id = aws_route_table.rt-2.id
}
