# SSH Remote Server Setup
Project URL: https://roadmap.sh/projects/ssh-remote-server-setup <br>

This project demonstrates how to set up a remote Linux server and configure SSH access using two different SSH key pairs.

## Steps
### 1. Setup Linux Server
* Ubuntu Server 22.04 LTS VM on local env

### 2. Generate two SSH key pairs
```bash
ssh-keygen -t ed25519 -f ~/.ssh/keyA
ssh-keygen -t ed25519 -f ~/.ssh/keyB
```
Substitute -t ed25519 with -t rsa -b 4096 or another variant.

### 3. Add Public Keys to Server
Use ssh-copy-id to install each public key into the server’s authorized_keys file.
```bash
ssh-copy-id -i ~/.ssh/keyA.pub user@server-ip
ssh-copy-id -i ~/.ssh/keyB.pub user@server-ip
```
Public keys will be appended to:
```bash
~/.ssh/authorized_keys
```

### 4. Connect Using SSH Keys
```bash
ssh -i ~/.ssh/keyA user@server-ip
ssh -i ~/.ssh/keyB user@server-ip
```
Both keys should authenticate successfully

### 5. SSH Config for Easier Login
Open the config file:
```bash
nano ~/.ssh/config
```
Add:
```bash
Host serverA
    HostName <server-ip>
    User <username>
    IdentityFile ~/.ssh/keyA

Host serverB
    HostName <server-ip>
    User <username>
    IdentityFile ~/.ssh/keyB

```
Now you can connect using:
```bash
ssh serverA
ssh serverB
```

### 6. Optional: Install Fail2Ban
Installed fail2ban to protect against brute force attacks:
```bash
sudo apt update
sudo apt install fail2ban
```
Enabled and check the status:
```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban
```
