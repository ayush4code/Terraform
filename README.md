Self - note

Monday, September 19, 2022
4:46 PM

Import - Configuration Language | Terraform | HashiCorp Developer

You can set pwd as env variable…or profile JSON file as credential 
![image](https://github.com/user-attachments/assets/26977549-4d46-4fed-861d-a76e2094d8bb)




Terraform.lock.hcl -- will lock provider while trying to init for downloading module
If suppose we share conf to other developer who tries to run on their env, where different provider is downloaded(latest one)..then it will throw error as it might not support API calls for some resources 

if you mention the provider version in config file - then the lock file will have version constraint( it will remember )
	- Now if you really want to change the provider ver and directly change the config file and run init, then u will get error - ( sol - run upgrade)
![image](https://github.com/user-attachments/assets/97bcb5ee-3013-40fd-8f86-58f6231817bd)



Arguments and attributes can be found in registry.terraform….
![image](https://github.com/user-attachments/assets/23bc15f7-a69c-4fea-87e3-775046f8c1ed)
![image](https://github.com/user-attachments/assets/b3f9012c-7a13-4979-93b3-54edc1541317)






Idempotent behavior----it won't change f nothing to change
![image](https://github.com/user-attachments/assets/7cf4e911-98c5-4631-9edf-0b65d64eda6f)


Terraform refresh will refresh state file from actual GCP (happens everytime before plan and apply)

![image](https://github.com/user-attachments/assets/53d313e7-e0d6-45e4-a836-d263d8e6a7ec)


Terraform.tfstate.lock.info (temp gets created when terraform apply command ongoing…it aquires lock)

The terraform force-unlock command can override the protections Terraform uses to prevent two processes from modifying state at the same time. You might need this if a Terraform process (like a normal apply) is unexpectedly terminated (like by the complete destruction of the VM it's running in) before it can release its lock on the state backend. Do not run this until you are completely certain what happened to the process that caused the lock to get stuck.


Terraform plan -lock=false  ( don't hold a lock during the operation….this will help if tfstate is in locked state --- generally happens if terminal was closed while apply was happening )

Terraform refresh -- it refreshes tf state from the actual AWS console….
then terraform plan -> it will show what changes would be done(after comparing with your code)

first ID is detected from state fie…then compared with console , then it sees tf file

Tarraform plan -target -- if only particular resource we want to refresh
Terraform plan -out = path 
Terraform destroy -auto-approve

Terraform refresh will add 1 serial number and then add counter to number of resources added….it backs up as well during the apply



From TF state file, only ID or name gets fetched while apply command(then reaching to AWS console for other details)….so this field we shouldn't change manually…..other fields even if changed, will get replaced while apply

Terraform.tfstate.backup
Gets reated only after 2nd apply..

Provisioner gets created only while creating resources..

Taint  -- resource get delete and recreated(only first time)…..taint happens if there was some error while creating a resource(even if resource gets created, but its marked as taint as the entire process was not completed)

Terraform apply - refresh="aws_instnce_web"

You can manually taint if refresh is recreation is needed after apply
![image](https://github.com/user-attachments/assets/1d382c3b-06fa-4e6e-b845-675d2d1aee8d)



If you replace the providers with new one, the older ones will get ignored(not tracked by Terraform anymore)
![image](https://github.com/user-attachments/assets/4c83b82d-fcc3-44ba-98ed-d5aeabd5ad65)





Terraform import 
Terraform state rm    --- to remove bad resource name from state file ( then reimport it…..refresh will only sync resource status whose ID is already in tfstate file)
Terraform state push/pull

Terraform state mv aws_instane.webapp aws_instance.myec2  ( command is used for renaming Terraform resource. Without this, resource will destroyed and recreated if we directly change resource name in confi.tf file)

Terraform state pull ( to showcase content of remote state file)



Command: state rm
The main function of Terraform state is to track the bindings between resource instance addresses in your configuration and the remote objects they represent. Normally Terraform automatically updates the state in response to actions taken when applying a plan, such as removing a binding for a remote object that has now been deleted.

You can use terraform state rm in the less common situation where you wish to remove a binding to an existing remote object without first destroying it, which will effectively make Terraform "forget" the object while it continues to exist in the remote system.



Recovering from State Disasters
If something has gone horribly wrong (possibly due to accidents when performing other state manipulation actions), you might need to take drastic actions with your state data.
• The terraform state pull command and the terraform state push command can directly read and write entire state files from and to the configured backend. You might need this for obtaining or restoring a state backup.


Count is not recommended - limitation : use only when a group needs to be created and destroys at same time. You can't delete just one within the group(eg - myvm2…it will replace 1 and remove 3rd vm)
![image](https://github.com/user-attachments/assets/aade24a3-b598-4fa4-9865-467aa1a2c829)



Use "for each" if resource life cycle is not together. You can delete middle reource(like vm2)

![image](https://github.com/user-attachments/assets/8cfe2e44-9578-4f4e-b3b8-f4be6615472d)




State file ( serial number , )

terraform init -migrate-state  ( used in case tfstate file name si changed manually )

![image](https://github.com/user-attachments/assets/3ac71f06-7e93-4519-ae83-13f1e7ad4d86)
![image](https://github.com/user-attachments/assets/54f6a68f-a348-4a8e-a147-0fe3844408e6)

![image](https://github.com/user-attachments/assets/2c357c20-9983-4536-9465-07fd484e8f26)





Once you do Terraform apply - it won't create state file and lock file locally…rather it can create in bucket (AWS S3 doen't support lock file)

![image](https://github.com/user-attachments/assets/279a490a-31be-41db-9178-146936d430a7)

![image](https://github.com/user-attachments/assets/455f5a63-0650-4f09-9170-b80e6932196d)
![image](https://github.com/user-attachments/assets/6ad61c05-047e-41cf-a0d0-9a14fad31283)











Workspace

GCP regitry:
https://github.com/hashicorp/terraform-provider-google

https://registry.terraform.io/modules/terraform-google-modules/iam/google/latest/submodules/service_accounts_iam


terraform/aws at master · admingagan/terraform · GitHub



Variable 

terraform plan -var 'project=<project_ID>'
terraform apply -var-file="prod_values"
terraform apply -var hw="t2.micro" -var name="gagan"


From .tfvars

From environment variable like TF_VAR_name
export TF_VAR_name="gagan-server3"


Declaration will be in main.tf or variable.tf
Value should be in variable.tfvars



If provider is missing, then instance is not tracked

Multiple provider
![image](https://github.com/user-attachments/assets/9f6df429-3848-42eb-a01c-cf1199f71456)


![image](https://github.com/user-attachments/assets/1e0335ff-63e7-485a-9348-954145f0c5bf)





Count

![image](https://github.com/user-attachments/assets/4e92e122-549d-4cc6-bf7a-3f7daf7f7be8)

![image](https://github.com/user-attachments/assets/48c847e2-fe1a-4b6c-a969-5f1fbc73c672)


![image](https://github.com/user-attachments/assets/a57e695b-1769-421b-b09b-c30c2a33dd75)

![image](https://github.com/user-attachments/assets/5b10f350-4ded-446a-b94c-5c1e974408d9)







Terraform Functions 



Main.tf


provider "aws" {
  region = "us-east-2"
  alias  = "useast2"
}

provider "aws" {
  region = "us-east-1"
  alias  = "useast1"
}

provider "aws" {
  region = "us-west-1"
  alias  = "uswest1"
}



Variable.tf

variable "instance_type" {
  default = "t2.micro"
}

variable "ami" {
  type = map
default = {
    us-east-1      = "ami-0c94855ba95c71c99"
    us-east-2      = "ami-0603cbe34fd08cb81"
  }
}



Resources.tf

resource "aws_instance" "myawsserver1" {
  ami = var.ami["us-east-1"]
  instance_type = var.instance_type
  provider = aws.useast1
  tags = {
    Name = "Techlanders-aws-ec2-instance1"
    Env = "Prod"
  }
}

output "myawsserver1-ip" {
  value = aws_instance.myawsserver1.public_ip
}

resource "aws_instance" "myawsserver2" {
  ami = var.ami["us-east-2"]
  provider = aws.useast2
  instance_type = var.instance_type

  tags = {
    Name = "Techlanders-aws-ec2-instance2"
    Env = "Prod"
  }
}

output "myawsserver2-ip" {
  value = aws_instance.myawsserver2.public_ip
}

======================================================================

For_each

provider "aws" {
  region = "us-east-2"
}

variable "server_names" {
  description = "Create virtual machines with these names"
  type        = list(string)
  default     = ["vm1", "vm2"]
}

resource "aws_instance" "myawsserver" {
  ami = "ami-0603cbe34fd08cb81"
  instance_type = "t2.micro"
  key_name = "test1"
  for_each = toset(var.server_names)
  tags = {
    Name = each.value
    env = "test"
  }
}

output "Private-IP" {
# As for_each loop is a map, you have to modify the syntax to get the values printed
 value = values(aws_instance.myawsserver)[*].private_ip
}



For-for_each

provider "aws" {
  region = "us-east-2"
}

variable "server_names" {
  description = "Create virtual machines with these names"
  type        = list(string)
  default     = [ "vm1", "vm3"]
}

variable "tags" {
  description = "map"
  type        = map(string)
  default     = {
    env      = "test"
    owner  = "keshav"
    dept   = "engineering"
    client = "techlanders-client99"
  }
}

resource "aws_instance" "myawsserver" {
  ami = "ami-0603cbe34fd08cb81"
  instance_type = "t2.micro"
  #key_name = "test1"
  for_each = toset(var.server_names)
  tags = {
   for key, value in var.tags :
    key => value
  }
}

output "Private-IP" {
# As for_each loop is a map, you have to modify the syntax to get the values printed
 value = {
  for instance in aws_instance.myawsserver:
    instance.id => instance.private_ip
 }
}

output "string-print" {
  value = [for first, second in var.tags : "${first} is having value ${second}"]
}


![image](https://github.com/user-attachments/assets/6063837a-19b2-41e0-bd3e-63ffb8a566cb)




variable

Wednesday, May 17, 2023
8:54 AM

![image](https://github.com/user-attachments/assets/374d1340-b663-468d-a4e5-890136a179d1)




Declaration will be in main.tf or variable.tf
Value should be in variable.tfvars

