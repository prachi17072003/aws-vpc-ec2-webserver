# AWS VPC + EC2 Web Server (Terraform)

Designed a web server inside a custom AWS VPC using Terraform.
**Status: design and plan only, not deployed** (validated with `terraform plan`).

## Architecture
![Architecture](architecture.png)

## What it builds
| Resource | Purpose |
|---|---|
| VPC (10.0.0.0/16) | Private network |
| Public subnet (10.0.1.0/24, ap-south-1a) | Holds the web server |
| Internet gateway | Gives the VPC internet access |
| Route table (0.0.0.0/0 -> IGW) | Sends outside traffic to the gateway |
| Route table association | Makes the subnet public |
| Security group | Allows HTTP (80) from anywhere, SSH (22) from one IP |
| EC2 (t3.micro) | Web server installed through user data |

## How I validated it
`terraform init`, `terraform validate` and `terraform plan` run in GitHub
Codespaces. Plan result: 7 to add, 0 to change, 0 to destroy.
![Plan](image.png)

## Key concepts
- [Why the subnet is public, in your own words]
- [Why SSH is limited to one IP]
- [What the security group is stateful for]

## Challenges
- Windows Application Control blocked the Terraform plugin on my laptop,
  so I ran Terraform in GitHub Codespaces instead.<img width="602" height="722" alt="architecture" src="https://github.com/user-attachments/assets/45e4f4a9-cdab-4ddd-a1bd-7cfc2720cbb6" />


## What I would do next
- Add a private subnet and NAT gateway
- Add an IAM role and S3 bucket
- Deploy it with `terraform apply` and destroy afterwards

## Tools
Terraform, AWS (VPC, EC2), GitHub Codespaces
