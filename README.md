## What is AWS Session Manager?

#### AWS Session Manager lets you securely connect and manage your servers or virtual machines (VMs) without opening network ports or using SSH keys. You can start remote sessions to your EC2 instances, on-premises servers, or VMs using a web browser or command-line tools.

---
## Problems with Traditional SSH
- You need to manage SSH key files.
- Port 22 must be open for SSH access (which is a security risk).
- Bastion hosts are often required for private instances.
- Security groups become harder to manage when allowing SSH access.

---
## Why Use Session Manager?
- No SSH keys needed.
- No inbound ports need to be open.
- No bastion host required.
- Sessions are secure and encrypted.
- Session activity can be logged for auditing.
