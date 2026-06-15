# Fixing SSH issues on cloned Ubuntu VMs

If your cloned VM isn't letting you SSH in—and you're seeing a "Connection reset by peer" error—it’s likely because the SSH server configuration was wiped out or corrupted during the cloning process. 

Since you can't reach it over the network, you'll need to log in directly through your hypervisor's console (like Proxmox, VMware, or VirtualBox) and run these steps to get it back online.

---

### 1. Fresh Install of SSH
First, let's remove any broken remnants of the old SSH service and do a clean install.

```bash
sudo apt update
sudo apt purge openssh-server -y
sudo apt install openssh-server -y
```

### 2. Generate New Host Keys
When cloning VMs, the most important thing to reset is the SSH Host Keys. If you don't reset them, the clone will share the exact same cryptographic identity as the original machine, which triggers security warnings.

Run these commands to delete the cloned keys and generate brand new, unique ones for this specific VM:

```bash
sudo rm /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server
```
### 3. Restart the Service
Finally, tell systemd to enable the service so it starts automatically when the VM boots up, and then turn it on right now:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

To verify everything is working correctly, check the status:

```bash
sudo systemctl status ssh
```
