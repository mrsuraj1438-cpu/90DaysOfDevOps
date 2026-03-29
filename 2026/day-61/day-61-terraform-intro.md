# Day 61 — I Learned Terraform Today and Made My First AWS Setup

## What I Did Today

Today I started learning **Terraform**. It is a tool that helps you create cloud resources using code. Before this, I used to create things in AWS by clicking buttons manually. Now I can do it with code!

---

## Task 1: What is Infrastructure as Code (IaC)?

### Simple meaning

Normally, if you want a server or storage in cloud, you go to AWS and click many buttons. This is manual work. Infrastructure as Code means you write code to do all that work automatically

### Why is it useful in DevOps?

* It saves a lot of time
* Less chances of making mistakes
* You can use same code again and again
* Team members can work together easily

### Problems with doing things manually in AWS

* It takes too much time
* You can make mistakes easily
* Hard to remember what you changed

### How IaC fixes these problems

* Everything becomes automatic
* You can save code in Git like normal code
* Easy to copy same setup in different places

### Terraform vs Other Tools

| Tool           | Type        | What it does best              |
| -------------- | ----------- | ------------------------------ |
| Terraform      | Declarative | Works with any cloud           |
| CloudFormation | Declarative | Works only with AWS            |
| Ansible        | Procedural  | Good for setting up software   |
| Pulumi         | Imperative  | Uses normal coding languages   |

### Two important words I learned

* **Declarative** — You just say *what you want*. Terraform figures out *how to make it*
* **Cloud-agnostic** — It works with AWS, Azure, Google Cloud and more

---

## Task 2: Install Terraform and Setup AWS

### Check Terraform is installed

```bash
terraform -version
```

### Setup AWS on my computer

```bash
aws configure
aws sts get-caller-identity
```

I checked and my AWS account was connected successfully ✅

---

## Task 3: Create a S3 Bucket Using Terraform

I made a file called `main.tf` and wrote this code:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-unique-terraform-bucket-12345"
}
```

### Commands I ran

```bash
terraform init
terraform plan
terraform apply
```

### What does `terraform init` do?

* It downloads the AWS plugin that Terraform needs
* It makes a folder called `.terraform/`

### What is inside `.terraform/` folder?

* The plugin files
* All the dependencies it needs
* Backend settings

---

## Task 4: Add an EC2 Instance (Virtual Server)

I added this to my `main.tf` file:

```hcl
resource "aws_instance" "my_instance" {
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"

  tags = {
    Name = "TerraWeek-Day1"
  }
}
```

### Commands I ran again

```bash
terraform plan
terraform apply
```

### How does Terraform know S3 bucket already exists?

Terraform has a **state file** called `terraform.tfstate`. This file remembers everything Terraform already created. So when I added the EC2, Terraform only created the EC2 and did not touch the S3 bucket again.

---

## Task 5: The State File

### Commands to check the state

```bash
terraform show
terraform state list
terraform state show aws_s3_bucket.my_bucket
terraform state show aws_instance.my_instance
```

### What does the state file save?

* The ID of every resource
* All the settings and details
* Extra information
* Which resources depend on which

### Why should I not edit the state file by hand?

* It can break everything
* Terraform will get confused about what is real

### Why should I not put state file in Git?

* It has secret information inside
* It can create a big security problem

---

## Task 6: Change Things and Delete Everything

### I changed the tag name

```hcl
tags = {
  Name = "TerraWeek-Modified"
}
```

### What do the symbols in `terraform plan` mean?

| Symbol | Meaning                        |
| ------ | ------------------------------ |
| +      | New resource will be created   |
| ~      | Existing resource will change  |
| -      | Resource will be deleted       |

### Result

Changing the tag only updated the resource, it did not delete and recreate it. This is called **in-place update (~)**.

### Delete everything

```bash
terraform destroy
```

Both S3 bucket and EC2 instance were deleted successfully ✅

---

## What I Learned Today

* Terraform makes it very easy to create and manage cloud things using code
* The state file is very important — Terraform uses it to track everything
* Always run `terraform plan` before `terraform apply` to see what will happen
* You can create and delete infrastructure anytime you want

---

## My Personal Thought

Before today, I thought DevOps is only about Docker and CI/CD pipelines. But today I learned that managing cloud infrastructure with tools like Terraform is also a very big part of DevOps. I feel more confident now!
