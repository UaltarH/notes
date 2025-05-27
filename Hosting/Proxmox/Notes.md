
# Restoring an LXC backup

**Always tick `Unique`** to avoid conflict in Mac addresses and always change **IP address** before starting a copy of another LXC

![[restore_lxc.png]]

# Mount an external volume in an LXC

## Synology NAS shared folder
### Adding Volume to Datacenter

This allows Proxmox to have access to your nas, which is useful for backup and also if you decide to create a container using the NAS resources. For example a service that required low specs or if you don't want your service that writes a lot to wear your SSD.

- In `Datacenter` > `Storage` > `Add`> `NFS`
-  Server: the NAS IP
- Export: `/volume1/<shared folder>`
- Content: **check all**
### Mounting
```bash
mount -t nfs 192.168.XX.XX:/volume1/<your shared folder> /mnt/<path to mount>

# or 
mount -t nfs -o rw 192.168.XX.XX:/volume1/<shared folder> /mnt/<shared folder>
```

### To create a volume from the NAS
First make sure you have [[Hosting/Proxmox/Notes#Adding Volume to Datacenter|added]] the NAS as storage.
- #### Option 1 : Via GUI
  
![[lxc_mount_volume.png]]

![[lxc_mount_point_creation.png]]
- #### Option 2 : Via CLI
  
In **Proxmox** shell edit a LXC config, to which you want to add a shared volume  at `/etc/pve/lxc/<CT ID>.conf`
add the following line :
```conf
mp0: nas:<CT ID>/vm-<CT ID>-disk-0.raw,mp=/mnt/proxmox,backup=1,size=8G
```
*Note* : If you want multiple LXCs to **share same volume** you can just copy the same line over to other LXC configs