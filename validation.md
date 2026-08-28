# Validation Notes

This file records the main checks performed during the lab. Resource identifiers are intentionally omitted or sanitized.

## EC2 web server

Apache was installed and enabled on the Amazon Linux EC2 instance:

```bash
sudo dnf install -y httpd
sudo systemctl enable --now httpd
systemctl status httpd
```

A basic test page was created under `/var/www/html/index.html` and successfully reached through the EC2 public endpoint over HTTP.

## IAM role identity

The EC2 instance was configured with an IAM role instead of static AWS credentials.

```bash
aws sts get-caller-identity
```

The returned ARN showed an assumed role session, confirming that the instance was receiving temporary role credentials.

## Private S3 access

The bucket remained private. Anonymous browser access to the object URL returned `AccessDenied`.

From EC2, the same bucket was accessible through the instance role:

```bash
aws s3 ls s3://<lab-bucket>
aws s3 cp s3://<lab-bucket>/migration-test.txt -
```

The tests were repeated after replacing broad S3 read permissions with a bucket-specific least-privilege policy.

## Network control validation

The EC2 Security Group used separate rules for public service access and administration:

```text
HTTP  TCP/80  0.0.0.0/0
SSH   TCP/22  <administrator-public-ip>/32
```

When the administrator public IP changed, the existing `/32` SSH rule no longer matched and the SSH connection timed out. Updating the rule to the current source IP restored access. This demonstrated source-based Security Group enforcement in practice.

## CloudTrail review

CloudTrail Event History was reviewed as the AWS control-plane audit source. Investigation fields reviewed included event name, event source, identity, event time, source IP address, Region, resources, and request details.
