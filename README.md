🔄 Cross-Platform Backup System
A cross-platform file sharing and automated backup system using NFS, rsync, and Samba, designed to sync data between Linux servers and provide access from Windows clients.
🧱 System Architecture
This system enables:
- Real-time file synchronization between two Linux servers using inotify + rsync
- File sharing to Windows clients via Samba
![System Architecture](https://github.com/Dingding-stack/cross-platform-backup/raw/main/architecture.png)
🧰 Requirements
The following tools are required on both Linux nodes:
- nfs-utils
- samba
- rsync
- inotify-tools

Install them using:

yum install -y nfs-utils samba rsync inotify-tools
![Requirements Screenshot](https://github.com/Dingding-stack/cross-platform-backup/raw/main/requirements.png)
🗂️ NFS Mount Configuration
Set up a shared directory on the server and mount it on the client using NFS.

### 1. Export directory on the server

Edit the /etc/exports file and add the following line:

/export/software *(rw,sync,no_root_squash)

Then apply the configuration and restart the NFS server:

exportfs -a
systemctl restart nfs-server

### 2. Mount on the client

mount -t nfs <server_ip>:/export/software /export/nfs_disk/

Replace <server_ip> with the actual IP address of your NFS server.
![NFS Mount Screenshot](https://github.com/Dingding-stack/cross-platform-backup/raw/main/nfs_mount.png)
🔁 Real-time File Sync with Rsync
We use rsync with inotify to monitor and sync files in real time from a source node to a backup node.

1. Configure rsync daemon on the receiving server (edit /etc/rsyncd.conf):

[node2_properties]
path=/all_server_data_log/data
comment = node2 properties
read only = no
auth users = backup_user
secrets file = /etc/rsyncd.password

Start rsync daemon:
rsync --daemon --config=/etc/rsyncd.conf

2. Write a sync script using inotifywait on the sending server.
![rsync Config Screenshot](https://github.com/Dingding-stack/cross-platform-backup/raw/main/rsyncd_conf.png)
![rsync Log Screenshot](https://github.com/Dingding-stack/cross-platform-backup/raw/main/rsync_log.png)
🪟 Windows Client Access
Windows users can easily access the shared directory on the Linux Samba server.

### Access via File Explorer

Open Windows Explorer and enter the following path in the address bar:

\\<Linux_Server_IP>\software

Replace <Linux_Server_IP> with the actual IP of your Samba server.

You may be prompted for credentials — use the Samba username and password you configured.
![Windows Samba Access Screenshot](https://github.com/Dingding-stack/cross-platform-backup/raw/main/samba_windows_mapped.png)
🚀 Possible Enhancements
Here are some features you can add to make the system more powerful and production-ready:
- 🔐 Add user access control and authentication via LDAP or Active Directory
- 🗃️ Use versioned or incremental backups (e.g., rsnapshot, BorgBackup)
- ☁️ Backup to cloud storage (e.g., AWS S3, Google Drive)
- 🌐 Add a simple web dashboard to view sync logs and system status
- 🧪 Use systemd to run the sync script as a background service
- 📅 Integrate email/Slack notifications on sync events or failures
🧠 Skills Demonstrated
This project showcases practical experience in:
- Linux system administration (NFS, Samba, rsync)
- Real-time file monitoring with inotify
- Secure file sharing and network configuration
- Shell scripting (automation, error handling)
- Cross-platform interoperability (Linux ↔ Windows)
- GitHub project documentation and demonstration
