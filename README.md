# terraform
## main.tf
```
provider "aws" {
  access_key = var.access
  secret_key = var.secret
  region = "ap-south-1"
}

resource "aws_instance" "myec2" {
  ami= "ami-0dee22c13ea7a9a67"
  instance_type = "t2.micro"
  key_name = aws_key_pair.mykey.key_name
  security_groups = [ aws_security_group.sg.name ]
  tags = {
    "Name" = "terraform_instance"
  }
  
  connection {
    type = "ssh"
    host = self.public_ip
    user = "ubuntu"
    private_key = file("/home/akash/.ssh/id_rsa")
  }

 provisioner "file" {
   source = "/home/akash/Desktop/terraform1/demo.txt"
   destination = "/home/ubuntu/demo.txt"
 }
provisioner "local-exec" {
  command = "echo ${aws_instance.myec2.public_ip} >myip.txt"
}
provisioner "remote-exec" {
  inline = [ "sudo apt update; sudo apt install apache2 -y" ]
  
}
}

resource "aws_key_pair" "mykey" {

  key_name = "jecrckey"
  public_key = file("/home/akash/.ssh/id_rsa.pub")
}

resource "aws_security_group" "sg" {
  name = "terra-sg"
  description = "terra-sg"
  vpc_id = "vpc-0043d7fa13a69e6dd"

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = [ "0.0.0.0/0" ]
  }
  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = [ "0.0.0.0/0" ]
  }
 egress {
  from_port = 0
  to_port = 0
  protocol = "-1"
  cidr_blocks = [ "0.0.0.0/0" ]
 }
}

output "publicip1" {
    value = aws_instance.myec2.public_ip
}
```

## var.tf

```
variable "access" {
    description = "this is access_key"
    default = "*****************"
  
}
variable "secret" {
  description = "this is secret_key"
  default = "********************************"
}
```
