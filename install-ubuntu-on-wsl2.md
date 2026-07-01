# WSL2 and Ubuntu Installation Guide
This guide covers installing the Windows Subsystem for Linux 2 (WSL2) and setting up an Ubuntu distribution.


## Prerequisites:
- Windows 11 or Windows 10 (Version 2004 or higher, Build 19041 or higher).
- **CPU Virtualization Enabled**: Must be turned on in your motherboard BIOS/UEFI settings (Intel VT-x or AMD-V/SVM).

### Steps
- Open powershell as administrator
Run this following command
```bash
wsl --install
```

-  Restart your computer when prompted to apply the system feature changes.
-  Check version by running this command
```bash
wsl --version
```

If the installation completed successfully you should see a version number (eg. 2.x.x)
- Install ubuntu by running this command
```bash
wsl --install -d ubuntu
```
Enter a new UNIX username and password when prompted. Once completed it is ready to use.

To ensure your environment is healthy and running on the correct version, open PowerShell and run:
```bash
wsl --list --verbose
```

Look for `Ubuntu` under the name column.
Verify the Version column displays `2`


### Troubleshooting
If you see the error `WSL2 is unable to start since virtualization is not enabled` then run the following command
```bash
wsl.exe --install --no-distribution
```
Then run again
```bash
wsl --install -d ubuntu
```

If issue persists, then run the following command
```bash
bcdedit /set hypervisorlaunchtype auto
```
Now retsart your computer. It should work now.
