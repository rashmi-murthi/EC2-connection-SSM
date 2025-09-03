
# ✅ AWS Session Manager Interview Q&A

**Intro:**  
This document contains AWS Session Manager Q&A covering conceptual understanding, practical steps, troubleshooting, and security best practices. It is useful for interview preparation and hands-on labs. For official documentation, see [AWS Session Manager Docs](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html).

## Conceptual / Configuration

**Q1. What is AWS Session Manager and how does it improve EC2 access security compared to SSH?**  
- Allows secure EC2 access without SSH keys or opening port 22.  
- Eliminates inbound security group rules.  
- Access logged in CloudTrail/CloudWatch.  
- Traffic encrypted via TLS.  
- Reduces attack surface (no exposed SSH).

**Q2. Which IAM policies are required for EC2 instances to work with Session Manager?**  
- Mandatory: `AmazonSSMManagedInstanceCore`  
- Optional: `CloudWatchLogsFullAccess` or custom policy for session logs.

**Q3. Why should you avoid assigning key pairs or opening port 22 for SSM-managed EC2s?**  
- Keys can be lost, leaked, or stolen.  
- Port 22 exposure increases attack surface.  
- SSM removes need for keys/ports → more secure.

**Q4. How do VPC endpoints enable EC2 access in private subnets without Internet?**  
- VPC Endpoints (SSM, EC2Messages, SSM Messages) allow private communication inside AWS network.  
- No Internet Gateway or NAT required.

**Q5. Prerequisites on EC2 for Session Manager to work?**  
- IAM role with `AmazonSSMManagedInstanceCore`.  
- SSM Agent installed and running.  
- Network access to SSM (Internet, NAT, or VPC Endpoints).

**Q6. How does Session Manager ensure encrypted and auditable access?**  
- TLS encryption for session data.  
- Logs session activity to CloudWatch Logs or S3.  
- API calls captured in CloudTrail.

**Q7. Supported operating systems?**  
- Amazon Linux / Amazon Linux 2  
- Ubuntu 16.04+  
- RHEL, CentOS, SUSE, Debian  
- Windows Server 2012 R2+

## Practical / Lab Workflow

**Q8. Connect via browser and CLI?**  
- **Browser:** EC2 Console → Connect → Session Manager → Start session.  
- **CLI:**  
```bash
aws ssm start-session --target <instance-id>
```

**Q9. Verify SSM Agent is running:**  
```bash
sudo systemctl status amazon-ssm-agent        # Amazon Linux
sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service  # Ubuntu
```

**Q10. Steps for enabling CloudWatch logging:**  
1. Create CloudWatch Log Group.  
2. Go to Systems Manager → Session Manager → Preferences.  
3. Enable CloudWatch/S3 logging and choose destination.  
4. Save settings.

**Q11. If EC2 has no outbound connectivity or VPC endpoints?**  
- Sessions fail.  
- Fix: Add NAT Gateway or VPC Endpoints.

**Q12. Additional security measures after enabling Session Manager:**  
- Remove SSH keys from EC2.  
- Delete inbound rules for port 22.  
- Enforce least privilege IAM policies for SSM.  
- Enable session logging.

## Troubleshooting / Advanced

**Q13. Troubleshoot if Session Manager can’t connect:**  
- Check IAM role (`AmazonSSMManagedInstanceCore`).  
- Verify SSM Agent is installed & running.  
- Ensure network path (Internet, NAT, VPC endpoints).  
- Confirm endpoints reachable.  
- Review CloudWatch/SSM logs.

**Q14. Can SSM work with on-premises or edge devices?**  
- Yes, via SSM Hybrid Activation → register on-prem/edge as managed instances.

**Q15. Restrict access to specific users via IAM:**  
- Use policies: `ssm:StartSession`, `ssm:TerminateSession`, `ssm:DescribeSessions`.  
- Apply conditions (tags, instance IDs).  
- Example: Only allow certain users on specific EC2 instances.

## Setup & Architecture

**Q16. Why no inbound rules or key pairs needed?**  
- EC2 initiates outbound connection to SSM service.  

**Q17. VPC Endpoint benefits:**  
- EC2 in private subnet reaches SSM without Internet/NAT.  
- Traffic stays in AWS backbone.

## Security & Best Practices

**Q18. Ensure SSM session logs are captured:**  
- Enable CloudWatch/S3 logging in Session Manager.  
- Configure IAM role for log delivery.

**Q19. Disable SSH after enabling Session Manager:**  
- No key pairs at launch.  
- Remove existing SSH keys.  
- Delete port 22 rules.

**Q20. How is session traffic encrypted?**  
- TLS in transit.  
- Optional KMS for logs encryption.

**Q21. Difference: VPC endpoints, NAT Gateway, public subnet for SSM?**  
- **Public subnet + IGW:** Internet (less secure).  
- **Private subnet + NAT:** EC2 → NAT → Internet.  
- **Private subnet + VPC Endpoints:** AWS backbone, no Internet (preferred).
