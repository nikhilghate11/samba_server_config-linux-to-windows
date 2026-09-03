# Samba Server Configuration

This repository contains the Samba server (`smb.conf`) configuration used for file sharing between Linux and Windows clients.

## 1. Install Samba

```bash
dnf install samba samba-client samba-common -y
```

## 2. Create Shared Directory

```bash
mkdir -p /samba/share
chmod 2770 /samba/share
```

## 3. Create Samba User

```bash
useradd sambauser
passwd sambauser
```

Add the user to the Samba database:

```bash
smbpasswd -a "sambauser"
```

Enable the Samba user:

```bash
smbpasswd -e sambauser
```

## 4. Configure Samba

Edit the Samba configuration file:

```bash
vim /etc/samba/smb.conf
```

Add:

```ini
[global]
    workgroup = WORKGROUP
    security = user

[share]
    path = /samba/share
    browsable = yes
    writable = yes
    valid users = sambauser
    read only = no
```

## 5. Check Configuration

```bash
testparm
```

If there are no errors, continue.

## 6. Start and Enable Samba

```bash
systemctl enable --now smb
systemctl enable --now nmb
```

Check status:

```bash
systemctl status smb
systemctl status nmb
```

## 7. Configure SELinux

```bash
chontext -t samba_share_t /samba/share
restorecon -Rv /samba/share
```

## 8. Configure Firewall

```bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload
```

Check:

```bash
firewall-cmd --list-services
```

## 9. Test Samba Share

List available shares:

```bash
smbclient -L localhost -U sambauser
```

Connect to the share:

```bash
smbclient //localhost/share -U sambauser
```

## 10. Client Access

From a Windows client:
go to serchbar and serch \\"ip of linux" and press enter
Then
give credential of samba user 
"samaba username"
"password"
Get access of linux share directory
```

## Important Files and Commands

| Item                  | Purpose                       |
| --------------------- | ----------------------------- |
| `/etc/samba/smb.conf` | Main Samba configuration      |
| `smbpasswd`           | Manage Samba users            |
| `testparm`            | Check Samba configuration     |
| `smbclient`           | Test/access Samba shares      |
| `smb.service`         | Samba service                 |
| `nmb.service`         | NetBIOS name service          |
| `samba_share_t`       | SELinux type for Samba shares |
| `firewall-cmd`        | Configure firewall            |

## Troubleshooting

Check Samba logs:

```bash
journalctl -u smb
```

Check service:

```bash
systemctl status smb
```

Check configuration:

```bash
testparm
```

Check SELinux:

```bash
ls -Zd /samba/share
```

Check firewall:

```bash
firewall-cmd --list-all
```
