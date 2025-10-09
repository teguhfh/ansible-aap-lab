# On NFS Server (aap1.tgh.lab)

1. Install NFS Server
    ```
    sudo dnf install -y nfs-utils
    ```

2. Enable and Start NFS Services
    ```
    sudo systemctl enable --now nfs-server rpcbind
    sudo systemctl start nfs-server rpcbind
    ```

4. Create and Export Directory (revised)
    ```
    # Create the pulp directory
    sudo mkdir -p /var/lib/pulp
    sudo mkdir -p /var/lib/pulp/pulpcore_static


    # Set appropriate permissions
    sudo chown -R root:root /var/lib/pulp
    sudo chmod 755 /var/lib/pulp
    ```

# Configure exports
```
sudo tee -a /etc/exports << EOF
/var/lib/pulp aap4.tgh.lab(rw,sync,no_root_squash,no_subtree_check)
/var/lib/pulp aap5.tgh.lab(rw,sync,no_root_squash,no_subtree_check)
/var/lib/pulp/pulpcore_static aap4.tgh.lab(rw,sync,no_root_squash,no_subtree_check)
/var/lib/pulp/pulpcore_static aap5.tgh.lab(rw,sync,no_root_squash,no_subtree_check)
EOF
```

# Apply exports
```
sudo exportfs -ra
```


3. Enable and Start NFS Services
```
sudo systemctl enable --now nfs-server rpcbind
sudo systemctl start nfs-server rpcbind
```

4. Configure Firewall
    ```
    sudo firewall-cmd --permanent --add-service=nfs
    sudo firewall-cmd --permanent --add-service=mountd
    sudo firewall-cmd --permanent --add-service=rpc-bind
    sudo firewall-cmd --reload
    ```