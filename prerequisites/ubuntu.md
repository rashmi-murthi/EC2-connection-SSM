# Ubuntu Prerequisites for AWS SSM

On Ubuntu, the SSM Agent might not be installed by default.  
Use the following script:

```bash
#!/bin/bash
sudo apt-get update
sudo snap install amazon-ssm-agent --classic
sudo systemctl start amazon-ssm-agent
sudo systemctl enable amazon-ssm-agent
```
