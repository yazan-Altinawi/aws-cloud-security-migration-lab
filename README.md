# AWS Cloud Security & Migration Lab

Hands-on AWS project focused on building and securing a small migration-style cloud environment using VPC, EC2, S3, IAM, and network security controls.

## What I Built

- VPC with two public and two private subnets across two Availability Zones
- Internet Gateway and S3 Gateway VPC Endpoint
- Amazon Linux EC2 web server running Apache
- Security Group with public HTTP and SSH restricted to a single /32 source
- Private S3 bucket with Block Public Access enabled
- EC2 IAM role for credential-free S3 access
- Bucket-specific least-privilege IAM policy

## Security Controls

- Network segmentation using public and private subnets
- Restricted SSH administration
- Private S3 storage
- IAM role instead of static AWS credentials
- Least-privilege S3 permissions
- No private keys or credentials stored in the repository

## Validation

- Connected to EC2 using SSH key authentication
- Hosted and accessed an Apache webpage over HTTP
- Confirmed anonymous S3 access was denied
- Verified EC2 assumed the IAM role
- Listed and read the private S3 object from EC2
- Re-tested access after reducing S3 permissions

## Production Improvements

For a production deployment, I would:
- use HTTPS/TLS on TCP 443
- remove direct public SSH where possible
- keep backend workloads in private subnets
- enable appropriate encryption, versioning, logging, and recovery
- continuously review IAM permissions

## Full Report

See the detailed technical report in:

`docs/AWS_Cloud_Security_Migration_Lab_Report.pdf`
