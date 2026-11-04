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
## Key concepts
- **Why the subnet is public:** its route table sends `0.0.0.0/0` traffic to the
  internet gateway, and the route table is associated with the subnet. Without
  that association the subnet would use the default route table and have no
  path to the internet.
- **Why SSH is limited to one IP:** port 22 gives remote control of the server,
  so I allow it from a single address (`/32`) instead of `0.0.0.0/0`. Opening
  SSH to the whole internet invites constant brute-force attempts. HTTP on
  port 80 is open to all because it is a public website.
- **What stateful means for the security group:** if a request is allowed in,
  the reply is allowed out automatically, so I don't write separate return
  rules. A NACL is stateless and would need rules in both directions.

## Challenges
- Windows Application Control blocked the Terraform plugin on my laptop,
  so I ran Terraform in GitHub Codespaces instead.<img width="602" height="722" alt="architecture" src="https://github.com/user-attachments/assets/45e4f4a9-cdab-4ddd-a1bd-7cfc2720cbb6" />


## What I would do next
- Add a private subnet and NAT gateway
- Add an IAM role and S3 bucket
- Deploy it with `terraform apply` and destroy afterwards

## Tools
Terraform, AWS (VPC, EC2), GitHub Codespaces
