# docker-with-s3fs-volume
1. Install s3fs and NFS Server:

Ensure s3fs and the NFS server are installed on your host machine.

```console

```

https://github.com/s3fs-fuse/s3fs-fuse

```console

```

2. Mount MinIO Bucket Using s3fs:

Add the following line to the password.txt file:
```text
minioadmin:minioadmin
```

```console
sudo mkdir -p /mnt/minio

s3fs minioadmin:minioadmin@localhost:9000 /mnt/minio -o passwd_file=/path/to/password.txt -o url=http://localhost:9000 

sudo s3fs my-bucket-name /mnt/minio -o passwd_file=/home/user/.s3creds,use_path_request_style,url=https://FQDN-hostname:9000

```

2. (optional) Share the Mounted Directory via NFS:

Install the NFS server if it's not already installed:
```console
sudo apt-get install nfs-kernel-server

sudo nano /etc/exports

```

Add the following line to share the /mnt/minio directory:
```text
/mnt/minio *(rw,sync,no_subtree_check)
```

Restart the NFS server to apply the changes:

```console
sudo systemctl restart nfs-kernel-server
```

3. Access the NFS Share

On the client machine, install the NFS client if it's not already installed:

```console
sudo apt-get install nfs-common
```

Mount the NFS share:
```console
sudo mount <server_ip>:/mnt/minio /path/to/mount/point
```