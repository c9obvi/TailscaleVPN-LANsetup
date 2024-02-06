# Shared Drive Access Setup Guide

This guide outlines the steps to set up shared drive access across workstations using Tailscale. It's designed to facilitate sharing firmware files for microcontrollers across your environment.

## Step 1: Set Up the Shared Folder on the Server

### Select or Create a Folder for Sharing

On the server or computer that will host the shared files, select an existing folder or create a new one for sharing. For instance, a folder named `Firmware`.

### Share the Folder

#### Windows

1. Right-click the folder, select `Properties`, then go to the `Sharing` tab.
2. Click `Advanced Sharing`, check `Share this folder`, and set a share name.
3. Click `Permissions` to set who can access the folder and their rights (read, write, etc.).
4. Apply the settings.

#### macOS

1. Open `System Preferences` > `Sharing`.
2. Check `File Sharing`, then click the `+` under `Shared Folders` to add your folder.
3. Add users under `Users` and set their access rights.

#### Linux

1. Install Samba if not already installed: `sudo apt install samba` (for Debian/Ubuntu).
2. Edit the Samba configuration file: `sudo nano /etc/samba/smb.conf`.
3. Add your shared folder at the end of the file:

![note] this is a BASH script
```[Firmware]
path = /path/to/your/folder
read only = no
browsable = yes
```

4. Restart Samba: `sudo systemctl restart smbd`.

## Step 2: Access the Shared Folder from Workstations

Ensure all workstations and the server hosting the shared folder are connected via Tailscale.

- **Windows**: Use File Explorer and access the shared folder by entering `\\{Tailscale_IP_of_server}\sharename` in the address bar.
- **macOS**: In Finder, select `Go` > `Connect to Server`, then enter `smb://{Tailscale_IP_of_server}/sharename`.
- **Linux**: Use the command line or your file manager. For the command line, you might mount the share using `mount -t cifs //${Tailscale_IP_of_server}/sharename /local/mount/point -o user=username`.

## Step 3: Configure Access Controls in Tailscale

1. Log into Tailscale Admin Console to manage your network's ACLs.
2. Modify ACLs to ensure they allow the necessary traffic between workstations and the server hosting the shared folder. You might need to add or adjust rules to explicitly allow connections to the server's Tailscale IP from other devices on your network.

## Step 4: Automation on the Server

For automating tasks like updating firmware files in the shared folder, consider using scripts or automation tools suitable for your server's operating system. For instance, you could use cron jobs on Linux or Task Scheduler on Windows to periodically check for new firmware versions and update the shared folder accordingly.

## Additional Tips

- **Security**: Make sure your shared folder permissions are correctly set to prevent unauthorized access.
- **Testing**: After setup, test accessing the shared folder from different workstations to ensure everything is configured correctly.
- **Documentation**: Document the IP addresses, share names, and access permissions for future reference and troubleshooting.
