![image](https://github.com/user-attachments/assets/51f0ead2-faff-4047-9ac9-ac8b9639fa77)


Even if you use password=sensitive, it will be visible in tfstate 




Git Module source 

Module "vpc" {
  source = "git::https://example.com/vpc.git?ref=v1.2.0
}

Or
Source = "git::ssh://username@example.com/storge.git


Note - by default Terraform will clone and use default branch (ref by HEAD ) in selected repository 
   It can be overwridded by ref 
==



Do not upload thesefiles in git 
Crash.log   -- If terraform crashes, then logs are stored here
.terraform -- this file gets created everytime we run init 


====

Terraform 1.5
Import 


=================


The terraform_remote_state Data Source | Terraform | HashiCorp Developer


The terraform_remote_state Data Source
![image](https://github.com/user-attachments/assets/062a46be-3251-4a8c-8d7c-19a395ff2c3b)




========================

Trace is the most verbose and it is default if TF_LOG is set to something other than a log level name 
export TF_LOG=TRACE

To persist log output
export TF_LOG_PATH=/tmp/terraform-crash.log


Terraform FMT


Terraform validate can check syntatically exp and ex - any variable missing, etc 

==============


Dynamic Block 
![image](https://github.com/user-attachments/assets/a3254f5f-2209-4f9b-bf11-e37cc5e23c9a)
![image](https://github.com/user-attachments/assets/3a7ce2e2-2e67-4ad9-9237-c88e004052ea)
![image](https://github.com/user-attachments/assets/897e95ac-50a0-4eac-a1e3-10e22f293e74)
![image](https://github.com/user-attachments/assets/1f8b9927-5189-4e63-8a7a-5dbc5fe3157c)



	




