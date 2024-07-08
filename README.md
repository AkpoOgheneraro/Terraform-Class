# Provision an AWS VPC using Terraform to create the following resources:
1. VPC
2. Subnets (public and private),
3. Internet Gateway,
4. Route Tables,
5. Security Groups,
6. Network ACLs (optional).


## Terraform code I executed on Vs code
**main.tf**:
```
Configure the AWS provider:
provider "aws" {
  region = "us-east-1" 
 
}


Create a VPC with a /16 CIDR block
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
}

# Create a public subnet in the first Availability Zone (modify as needed)
resource "aws_subnet" "public_subnet" {
  vpc_id            = aws_vpc.my_vpc.id
  cidr_block          = "10.0.1.0/24"
  availability_zone  = "us-east-1a"

  # Map the public subnet to a public route table (created later)
  map_public_ip_on_launch = true
}

# Create a private subnet in another Availability Zone (modify as needed)
resource "aws_subnet" "private_subnet" {
  vpc_id            = aws_vpc.my_vpc.id
  cidr_block          = "10.0.2.0/24"
  availability_zone  = "us-east-1b"
}

# Create an internet gateway
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.my_vpc.id
}

# Create a public route table for the public subnet
resource "aws_route_table" "public_route_table" {
  vpc_id = aws_vpc.my_vpc.id
}

# Create a private route table for the private subnet
resource "aws_route_table" "private_route_table" {
  vpc_id = aws_vpc.my_vpc.id
}

# Associate the public subnet with the public route table
resource "aws_route_table_association" "public_rt_association" {
  subnet_id         = aws_subnet.public_subnet.id
  route_table_id    = aws_route_table.public_route_table.id
}

# Associate the private subnet with the private route table
resource "aws_route_table_association" "private_rt_association" {
  subnet_id         = aws_subnet.private_subnet.id
  route_table_id    = aws_route_table.private_route_table.id
}

# Create a public route table for the public subnet
resource "aws_route_table" "Nexa_route_table" {
  vpc_id = aws_vpc.my_vpc.id
}


# (Optional) Create security groups for your public and private subnets
resource "aws_security_group" "public_sg" {
  name = "public_security_group"
  
}

resource "aws_security_group" "private_sg" {
  name = "private_security_group"
  
}

# (Optional) Create Network ACLs for your subnets
resource "aws_network_acl" "public_nacl" {
  vpc_id = aws_vpc.my_vpc.id
 
}

resource "aws_network_acl_association" "public_nacl_association" {
  subnet_id = aws_subnet.public_subnet.id
  network_acl_id = aws_network_acl.public_nacl.id
}
```



