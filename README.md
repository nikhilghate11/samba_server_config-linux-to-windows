# Samba Server Configuration

This repository contains the Samba server (`smb.conf`) configuration used for file sharing between Linux and Windows clients.

## Usage
To test or deploy this configuration:
1. Validate syntax: `testparm -s smb.conf`
2. Copy to Samba directory: `sudo cp smb.conf /etc/samba/smb.conf`
3. Restart Samba: `sudo systemctl restart smb nmb`
