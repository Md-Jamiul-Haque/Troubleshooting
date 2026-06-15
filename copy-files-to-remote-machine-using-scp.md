# Copy files using SCP

Using scp (Secure Copy Protocol) is one of the quickest and most secure ways to transfer files between two machines, as it runs over standard SSH.

Here is exactly how to do it, depending on the direction you want the file to go.

### 1. The Core Syntax
The basic formula for scp always follows this structure:

```bash
scp [options] [source] [destination]
```

To connect to a remote machine, you will need its username and IP address (or hostname).

### 2. Transferring From Your Local Machine to a Remote Machine
If you are sitting at Machine A and want to push a file to Machine B, use this command:

```bash
scp /path/to/local/file.txt username@remote_ip:/path/to/remote/directory/
```

### Example:
```bash
scp ~/Documents/report.pdf ubuntu@192.168.1.50:/home/ubuntu/Downloads/
```

### 3. Transferring From a Remote Machine to Your Local Machine
If you are sitting at Machine A and want to pull a file from Machine B down to your current machine:

```bash
scp username@remote_ip:/path/to/remote/file.txt /path/to/local/directory/
```

Example:
```bash
scp ubuntu@192.168.1.50:/home/ubuntu/backups/db.sql ~/Desktop/
```
(Note: The `.` at the very end of a command can also be used to mean "drop it right here in my current working directory".)

### 4. Transferring Entire Directories (Folders)
By default, scp only copies individual files. If you try to copy a folder, you'll get an error. To copy an entire directory, simply add the -r (recursive) flag right after scp.

```bash
scp -r ~/Projects/ ubuntu@192.168.1.50:/home/ubuntu/
```

### For custom SSH Ports
If your remote machine uses a non-standard SSH port (something other than 22), use the uppercase -P flag to specify it:

```bash
scp -P 2222 file.txt username@remote_ip:/remote/dir/
```

### SSH Keys
If you normally log into the remote machine using an SSH key instead of a password, scp will automatically use it. If you need to specify a specific private key, use the -i flag:

```bash
scp -i ~/.ssh/my_key.pem file.txt username@remote_ip:/remote/dir/
```
>Prerequisite: For this to work, the destination machine must have the SSH server installed and running. If it isn't, you can install it on the destination machine using:

```bash
sudo apt update && sudo apt install openssh-server -y
```
