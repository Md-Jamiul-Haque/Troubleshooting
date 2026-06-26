# Setting a Static IP on Ubuntu
Ubuntu uses Netplan for network configuration. It relies on simple YAML files to get the job done. Here is how to lock down your IP address in under five minutes.

### Step 1: Find Your Network Interface Name
Before changing anything, you need to know the name of your network card (interface) and your current gateway. Run this command:

```bash
ip r
```
or
```bash
ip a
```
Look for the line starting with default via. It will give you two crucial pieces of info:
- Your Gateway IP (e.g., 192.168.1.1)
- Your Interface Name (e.g., ens33 or eth0)

### Step 2: Locate Your Netplan File
Netplan configuration files live in the `/etc/netplan/` directory. List them using:

```bash
ls /etc/netplan/
```
You’ll usually see a file named something like `00-installer-config.yaml` or `50-cloud-init.yaml`.

### Step 3: Edit the YAML Configuration
Open the file with nano (or your preferred editor):

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```
Clear out the existing configuration and replace it with the structure below.

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:                             # replace ens33 with your interface name
      dhcp4: no
      addresses:
        - 192.168.1.10/24              # replace with your preferred IP address
      routes:
        - to: default
          via: 192.168.1.1             # replace with your default gateway
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]  # replace with your preferred DNS servers
```

> **NOTE:** YAML is incredibly strict about spacing. Use proper indentation.

### Step 4: Test and Apply Changes
Run this command to test your configuration:
```bash
sudo netplan try
```
If you made a typo or formatting error, it will automatically roll back the changes after a countdown so you don't accidentally lock yourself out.
If everything looks good and your connection holds, press Enter to accept.

Alternatively, if you are directly on the machine and confident, you can apply it instantly:
```bash
sudo netplan apply
```

### Step 5: Verify
Confirm your machine is using the new static IP:
```bash
ip r
```

You are all set! Your Ubuntu machine now has a permanent, static IP address on your network.
