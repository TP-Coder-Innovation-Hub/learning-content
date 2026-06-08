# Infrastructure as Code (IaC)

## What

Infrastructure as Code means defining your infrastructure (servers, networks, databases, configs) in code files instead of clicking through a web console. You version, review, and apply infrastructure changes the same way you manage application code.

## ClickOps vs IaC

### ClickOps

You create resources through a web console. Click, type, click, next, create.

Problems:
- Not repeatable — you cannot recreate the exact same setup
- Not reviewable — no one saw what you clicked
- Not documented — in 6 months, no one remembers why that port is open
- Not recoverable — if the account is lost, the infrastructure is gone

### IaC

You write a file that describes your infrastructure. You apply the file. The platform creates the resources.

```
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

Benefits:
- Repeatable — apply the same file to create identical environments
- Reviewable — pull requests for infrastructure changes
- Documented — the code is the documentation
- Recoverable — recreate everything from the files

## Terraform Concepts

Terraform is the most widely used IaC tool.

### Provider

A plugin that knows how to talk to a specific cloud or service.

```
provider "aws" {
  region = "us-east-1"
}
```

Providers exist for AWS, Azure, GCP, Kubernetes, GitHub, Datadog, and hundreds more.

### Resource

A piece of infrastructure you want to create.

```
resource "aws_s3_bucket" "data" {
  bucket = "my-app-data-bucket"
}
```

### State

A file that maps your code to real-world resources. Terraform uses state to know what it has already created and what needs to change.

State is critical. Lose the state file, and Terraform thinks nothing exists. Store state remotely (S3 + DynamoDB for locking, Terraform Cloud, or your cloud's state storage).

### Plan

Before making changes, Terraform shows you what it will do:

```
$ terraform plan

  + aws_s3_bucket.data will be created
  + aws_s3_bucket_ownership_controls.data will be created

Plan: 2 to add, 0 to change, 0 to destroy.
```

Always review the plan. "2 to destroy" when you expected "2 to add" is a bad day.

### Apply

Execute the plan.

```
$ terraform apply
```

## Workflow

1. Write or modify Terraform code
2. Run `terraform plan` — review the changes
3. Run `terraform apply` — create or update resources
4. Commit the code and state changes

In teams:
1. Create a branch with infrastructure changes
2. Open a pull request
3. CI runs `terraform plan` and posts the output
4. Team reviews the plan
5. Merge triggers `terraform apply`

## Why IaC

- **Consistency** — Dev, staging, and prod are identical (same code, different variables)
- **Speed** — Create an entire environment in minutes, not days
- **Safety** — Review changes before applying. Roll back by applying an older version
- **Collaboration** — Multiple engineers can work on infrastructure without stepping on each other

## Common Mistakes

- Editing resources in the console after provisioning with IaC. The next apply will overwrite or conflict with manual changes. If you use IaC, use it for everything.
- Not locking state. Two people applying simultaneously can corrupt state. Use state locking.
- Storing secrets in Terraform variables. Use a secrets manager and reference it from Terraform.
- Not running `terraform plan` before `apply`. Always review the plan.
