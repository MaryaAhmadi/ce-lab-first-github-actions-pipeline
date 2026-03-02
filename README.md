# Lab M5.01 - First GitHub Actions Pipeline

**Module:** Cloud Automation & CI/CD

![Terraform CI](https://github.com/MaryaAhmadi/ce-lab-first-github-actions-pipeline/actions/workflows/ci.yml/badge.svg)

## Overview
This repository demonstrates a GitHub Actions CI pipeline for Terraform.  
The pipeline automatically runs:

- **terraform fmt -check** — formatting check  
- **terraform validate** — validates configuration syntax  
- **terraform plan** — generates an execution plan  

on every push and pull request, ensuring that infrastructure code is well-formed before merging into `main`.

## Pipeline Steps
1. **Checkout** — clones the repository into the GitHub Actions runner  
2. **Setup Terraform** — installs Terraform version 1.6.0  
3. **Format Check** — runs `terraform fmt -check` recursively  
4. **Init** — initializes Terraform providers (backend disabled during CI)  
5. **Validate** — validates configuration syntax  
6. **Plan** — generates an execution plan without applying changes  

## Terraform Resources
- **S3 Bucket** with:
  - Versioning enabled
  - Server-side encryption (AWS KMS)
  - Public access blocked
- **Lifecycle Rules** (added in feature branch):
  - Transition objects to cheaper storage classes (`STANDARD_IA`, `GLACIER`)
  - Expire old versions after 90 days  

## How to Use Locally
```bash
# Clone the repository
git clone https://github.com/MaryaAhmadi/ce-lab-first-github-actions-pipeline.git
cd ce-lab-first-github-actions-pipeline

# Initialize Terraform
terraform init

# Run local checks
terraform fmt -check
terraform validate
terraform plan


## Required Secrets

To run the GitHub Actions workflow, the following secrets must be set in your repository:

|       Secret       |	  Description   |
 AWS_ACCESS_KEY_ID   |  IAM access key   |
AWS_SECRET_ACCESS_KEY	| IAM secret key    |

## Notes

- Workflows run automatically on push and pull requests to main

- No Terraform state is stored during CI; the backend is disabled for safety

- Pull Requests will show a green check when all Terraform steps pass successfully


## Submission Instructions

1. Fork this repository to your GitHub account

2. Clone your fork locally:

git clone https://github.com/<your-github-username>/ce-lab-first-github-actions-pipeline.git
cd ce-lab-first-github-actions-pipeline


Complete the lab in your fork: create branches, add resources, and open pull requests

After finishing, commit and push all changes:

git add .
git commit -m "Complete lab M5.01"
git push origin main



✅ Key Takeaways

GitHub Actions workflows live in .github/workflows/ and are version-controlled alongside your code

CI pipelines catch formatting, validation, and syntax errors before code reaches main

terraform fmt -check enforces consistent style without modifying files

terraform validate catches syntax and provider errors without needing cloud credentials

Secrets keep sensitive values out of codebase