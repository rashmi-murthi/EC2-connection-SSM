## SSM Agent Preinstalled in AMIs
- ✅ **Amazon Linux 2 (and Amazon Linux 2023+)** → SSM Agent installed and running by default.  
- ✅ **Ubuntu 16.04, 18.04, 20.04, 22.04 (LTS) and newer** → SSM Agent included as a snap package.  
  

## Verify SSM Agent on Linux (Amazon Linux / Ubuntu)
To verify:
```bash
sudo systemctl status amazon-ssm-agent # Amazon Linux
sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service # ubuntu
```
If it shows `active (running)`, the agent is ready.

## Windows Prerequisites for AWS SSM - AWS CLI & Session Manager Plugin 
Install AWS CLI and Session Manager plugin using Chocolatey:

```powershell
choco install awscli
choco install awscli-session-manager
```

To verify: 
```powershell 
aws --version
session-manager-plugin --version 
```
