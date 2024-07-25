terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.59.0"
    }
  }
}

provider "aws" {
    region = var.region
}

resource "aws_instance" "myinstance" {
    ami           = "ami-00db8dadb36c9815e"  // Specifies the Amazon Machine Image (AMI) to use for the instance
    instance_type = "t2.micro"               // Specifies the instance type (in this case, t2.micro)

    tags = {                                 // Tags to assign to the instance
        Name = "myinstance"                  // Sets the 'Name' tag to 'myinstance'
    }
}
