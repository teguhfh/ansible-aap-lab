On NFS Clients (aap4.tgh.lab and aap5.tgh.lab)
1. Install NFS Client

```
sudo dnf install -y nfs-utils
```

2. Create Mount Point (as root)
```
sudo mkdir -p /var/lib/pulp
```

3. Configure Mount

# BASIC Add to fstab for persistent mounting
```
echo "aap1.tgh.lab:/var/lib/pulp /var/lib/pulp nfs rw,sync,hard,intr 0 0" | sudo tee -a /etc/fstab
```

3. Configure fstab with SELinux Contexts

# Backup original fstab
sudo cp /etc/fstab /etc/fstab.backup

# Add the mount entries with SELinux contexts
sudo tee -a /etc/fstab << EOF
aap1.tgh.lab:/var/lib/pulp /var/lib/pulp nfs defaults,_netdev,nosharecache,context="system_u:object_r:var_lib_t:s0" 0 0
aap1.tgh.lab:/var/lib/pulp/pulpcore_static /var/lib/pulp/pulpcore_static nfs defaults,_netdev,nosharecache,context="system_u:object_r:httpd_sys_content_rw_t:s0" 0 0
EOF


# Mount the filesystem
```
sudo systemctl daemon-reload
sudo mount /var/lib/pulp

sudo mount -a
```

4. Verify Mount

# Check if mounted correctly
```
mount | grep pulp
df -h | grep pulp

```

# Test write permissions (as root since pulp user doesn't exist yet)
```
sudo touch /var/lib/pulp/test_file
sudo rm /var/lib/pulp/test_file
```