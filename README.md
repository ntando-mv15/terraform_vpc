# Introduction to Terraform: Configuring a VPC and Webserver

## Overview
This documentation outlines the content I covered in an introductory crash course on Terraform provided by FreeCodeCamp.org on YouTube. The course aims to provide learners with foundational knowledge and practical skills in using Terraform for infrastructure provisioning and management. I completed the course in three days, covering essential Terraform concepts, commands, and practical exercises.

My motivation for learning Terraform stems from a dual purpose: firstly, to bolster my skills in cloud computing, particularly within the AWS ecosystem, driven by the desire to excel in projects like the AWS Cloud Resume Challenge, which demands infrastructure management using Terraform. Secondly, I recognize the broader utility of Terraform as a tool for personal skill development, enabling automation of infrastructure deployment and configuration in line with industry standards and practices. This endeavor serves not only to enhance my AWS proficiency but also to cultivate expertise in cloud architecture and infrastructure management, paving the way for future career opportunities in cloud computing and software engineering.

### Course Instructor
- Sanjeev Thiyagarajan from FreeCodeCamp.org
- [Course Link](https://video.search.yahoo.com/search/video;_ylt=AwrFGTwAEPRl2z4A195XNyoA;_ylu=Y29sbwNiZjEEcG9zAzEEdnRpZAMEc2VjA3Nj?type=E210US885G0&p=Terraform+Course+-+Automate+your+AWS+cloud+infrastructure&fr=mcafee&turl=https%3A%2F%2Ftse4.mm.bing.net%2Fth%3Fid%3DOVP.RsD0woNeX_MUVk8TdFIWYAHgFo%26pid%3DApi%26w%3D296%26h%3D156%26c%3D7%26p%3D0&rurl=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DSLB_c_ayRMo&tit=Terraform+Course+-+Automate+your+AWS+cloud+infrastructure&pos=01&vid=c41440587d41342914f6668c70f6cac7&sigr=8gHx5VoAp8M0&sigt=d6uVyDH_HFLi&sigi=n.GNYu8pVhMh)

## Day 1: Introduction to Terraform

### Topics Covered
1. Terraform Providers
2. Terraform Documentation
3. Terraform Files
   - .tf files
   - State file
   - lock.hcl
4. Main Terraform Commands
   - init
   - plan
   - apply
   - destroy

### Understanding Terraform Providers:

I learned about Terraform providers, which are responsible for interacting with APIs to manage resources such as virtual machines, databases, and networking components within various cloud providers or infrastructure platforms. Which provider is used, is project specific.

When making use of the AWS provider, you have to set up authentication to securely connect Terraform to AWS service and resources. This is done by set the AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables with your access key ID and secret access key, respectively. Terraform will automatically use these environment variables for authentication.


```
export AWS_ACCESS_KEY_ID=your_access_key_id
export AWS_SECRET_ACCESS_KEY=your_secret_access_key
```
Hard coding our access keys is not recommended as it poses security risks. 

### Terraform Files:

These files play crucial roles in defining infrastructure configuration, storing state information, and managing locking mechanisms for concurrent operations.

- .tf files: 
These are Terraform configuration files where you define the infrastructure components, such as resources and their configurations, using a declarative syntax.

- State file: 
This file keeps track of the current state of your infrastructure as managed by Terraform. It stores information about the resources Terraform has provisioned and their current state.

- lock.hcl: 
This file provides a locking mechanism to prevent concurrent operations that may lead to conflicts or inconsistencies in the Terraform state. It helps maintain the integrity of the state file during collaborative work or parallel executions.

### Terraform Documentation

I learned how to navigate and utilize the Terraform documentation specific to AWS, enabling me to understand the syntax and structure required to declare infrastructure components effectively.

### Main Terraform Commands:

I familiarized myself with the primary Terraform commands: init, plan, apply, and destroy. These commands are essential for initializing Terraform configurations, generating execution plans, applying changes to infrastructure, and destroying provisioned resources, respectively. Once we have our Terraform configuration declared in our .tf file, you run these commands on your terminal step by step. 

1. Initialize Terraform configurations and preparing it for use by downloading necessary plugins and modules defined in the configuration files, we use: 

```
terraform init
```

Generate execution plan, and check what actions Terraform will take when you apply the configuration, such as creating, updating, or destroying resources:

```
terraform plan
```
3. Apply the changes specified in the Terraform configuration to the target infrastructure, creating, updating, or destroying resources as necessary according to the execution plan generated, we use:

```
terraform apply
```

To destroy infrastructure provisioned:
```
terraform destroy
```

Note: If you make changes to your code, you can save the changes on the .tf and rerun terraform plan and terraform apply.


## Day 2: Advanced Terraform Concepts and Mini Project

### Topics Covered

1. More Terraform Commands (Terraform State Commands, Terraform Refresh)
2. Defining Outputs
3. Terraform Variables
4. Terraform Targets
5. Checking the State File



### More Terraform Commands:

I explored additional Terraform commands for managing infrastructure effectively.

To list all the resources tracked by Terraform in the state file on your terminal. It provides an overview of the current state of your infrastructure.

```
terraform state list
```

To display the attributes and current state of a specific resource managed by Terraform. It helps you inspect the details of individual resources.

```
terraform state show
```

### Defining Outputs: 

In Terraform configuration files, you can define outputs to specify which information you want to extract from your infrastructure. Outputs can include attributes of resources, computed values, or any other relevant data.

For example, if you want to extract information about your server ip:

```hcl
output "server_public_ip" {
  value = aws_eip.one.public_ip
}

```

### Terraform Refresh 

When you run terraform refresh, Terraform will query the infrastructure provider (e.g., AWS, Azure, GCP) to get the latest state of all resources managed by your Terraform configuration. It compares this real-world state with the state recorded in the Terraform state file. If there are any differences, it updates the state file to reflect the current state of the infrastructure. 

For example, 


### Terraform Targets

Terraform targets allow you to focus on specific resources or modules within your Terraform configuration when applying commands like plan, apply, or destroy. By specifying targets, you can target an individual resource or a few resources you're interested in, rather than applying changes to the entire infrastructure defined in your configuration files. 

For example, to delete just this one resource(web instance):

```
terraform destroy -target "aws_instance" "web"
```

### Terraform Variables:

You can use variables to represent different kinds of information, like names, numbers, or lists, and change them whenever you need to without rewriting your code. Overall, variables make your Terraform configurations more adaptable and easier to manage

```hcl
variable "region" {
  description = "The AWS region where resources will be created"
  default     = "us-east-1"
  type = string
}
```

You can change the value by running this command, which would override the default value declared in the code:

```
terraform apply -var="region=us-east-1"
```

Best practice however, is to create a variable file, store and makes changes to the value of your variables there. Terraform looks for a file named "terraform.tfvars" for variable assignment. The variable should still be declared in the main .tf file as well.

In the terraform.tfvars file:

```hcl
region = "us-east-1"
```

You can multiple .tfvar files for variable if your code gets more complex. Here, you can reference a specific .tfvar file when applying changes. 

```
terraform apply -var-file example.tfvars
```


### Checking State File:

I understood the importance of the state file in Terraform and learned how to inspect and manage it. This knowledge is crucial for understanding the current state of provisioned infrastructure and ensuring consistency in Terraform operations.

## Day 3: Practical Project: Deploying a VPC with Components

### Topics Covered
1. Practice Project: Deploying VPC
   - Public Subnet
   - Internet Gateway
   - Route Table
   - Association of Route Table with Subnet
   - Network Interface Card (NIC)
   - Deploying a Web Server with User Data for Apache Setup


## Mini Project: Deploying VPC

I engaged in a hands-on project where I deployed a Virtual Private Cloud (VPC) along with its associated components, including public subnets, internet gateways, route tables, and network interface cards (NICs). This practical exercise provided a real-world scenario to apply Terraform concepts.

 - [Project Code Link](https://github.com/ntando.mv15/terraform_vpc/blob/main/main.tf)


Through the project, I gained valuable hands-on experience using Terraform to provision and manage infrastructure. This practical experience reinforced my understanding of Terraform concepts and workflow. Refer to the images below:

![Screenshot 2024-03-13 073846](https://github.com/ntando-mv15/terraform_learn/assets/88146095/d16ead65-cdb6-4f72-a41a-97c74c9f7861)

![Screenshot 2024-03-13 073658](https://github.com/ntando-mv15/terraform_learn/assets/88146095/ff266918-e992-4adc-b474-0b48737628d2)

![Screenshot 2024-03-13 073735](https://github.com/ntando-mv15/terraform_learn/assets/88146095/271e483f-b42f-4d93-abda-bdda96264bb5)

![Screenshot 2024-03-15 105943](https://github.com/ntando-mv15/terraform_learn/assets/88146095/883eb488-fe19-458e-b2a0-6b837e86f4d4)




