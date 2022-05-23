Topics Covered Today

1) Infrastructure as Code 

---> main.tf (Create a instance using terraform)
provider "aws" {
  region     = "us-east-2"
}

resource "aws_instance" "webserver" {
  ami                    = "ami-019a4607ba39bfde6"
  instance_type          = "t2.micro"
    Name = "webserver"
  }
}


---> ec2.sh (Create instance using shell script)
 #!/bin/bash

EC2_INSTANCE=$(ec2-run-instances --instance-type t2.micro ami-0edab436f892279)

INSTANCE=$(echo ${EC2_INSTANCE} | SED 's/*INSTANCE //' | SED ' s/ -*//')

# Wait for the instance to be ready
while ! ec2-describe-instances $INSTANCE | grep -q "running"
do 
     echo waiting for $INSTANCE is to be ready...
done

# Check if instance is not provisioned and exit
if [ ! $(ec2-describe-instances $INSTANCE | grep -q "running") ]; then
  echo Instance $INSTANCE is stopped.
  exit
fi

ec2-associate-address $IP_ADDRESS -i  $INSTANCE

echo INstance $INSTANCE was craeted successfully!!!

--> ec2.yaml (create a instance using yaml )

 - amazon.aws.ec2:
       key_name: mykey
       instance_type: t2.micro
       image: ami-0edab436f892279
       wait: yes
       group: webserver
       count: 3
       vpc_subnet_id: subnet_29e53245
       assign_public_ip: yes

2) Provisioning Tools
    --> Deploy immutable infrastructure resources
    --> Servers, Database, Netawork Components
    --> Multiple Providers


3) HashiCorp Configuration Language
     --> HCL file consists of  Blocks and Arguments
     --> Blocks (file name )
               <block> <parameters> {
                    key1 = value1
                    key2 = value2
           ex - resource "local file" "pet"
     --> Arguments (content)
                   filename = "/root/pets.txt"
                   content = "write a content!"

4. Terraform META-ARGUMENTS
    --> Create before destroy
    --> Prevent destroy
    --> Ignore changes

5. Terraform Variables
    --> Terraform input variable - Input variables serve as parameters for a Terraform module, allowing aspects of the module to be customized without altering the module's own source code, and allowing modules to be shared between different configurations.
    --> Terraform output variable - Output values are like the return values of a Terraform module and have several uses
    --> Terraform local values - A local value assigns a name to an expression, so you can use that name multiple times within a module without repeating it.


        
