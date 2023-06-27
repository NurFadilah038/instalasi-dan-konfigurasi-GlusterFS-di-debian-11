# instalasi-dan-konfigurasi-GlusterFS

IP address tiap node :
Master =10.2.15.209
Client-1 =10.2.15.57
Client-2 = 10.2.15.58


Lakukan perintah-perintah berikut pada Master dan semua client
-----------------------------------------------------------------------------------------
#apt -y install glusterfs-server
#systemctl enable --now glusterd
#gluster --version
#lvm version
#apt-get install lvm2
#echo $PATH
#pvcreate /dev/sdb atau fdisk -l /dev/sdb
#vgcreate vg_glusterfs /dev/sdb
#lvcreate -l100%FREE -n lv_gluster vg_glusterfs
#mkfs.ext4 /dev/vg_glusterfs/lv_gluster
#systemctl start glusterd
#mkdir -p /data/gluster
#echo /dev/vg_glusterfs/lv_gluster /data/gluster ext4 defaults 0 0 >> /etc/fstab
#mount -a

Jika mount -a tidak berhasil lakukan perintah berikut jika berhasil skip :
#file -s /dev/vg_glusterfs/lv_gluster
#apt-get install ext4progs

#mkdir -p /data/gluster/brick1
#df -h

Master 
---------
#Gluster peer probe 10.2.15.57
#Gluster peer probe 10.2.15.58
#Gluster volume create gluster_vol replica 3 10.2.15.209:/data/gluster/brick1 10.2.15.57:/data/gluster/brick2 10.2.15.58:/data/gluster/brick3
#Gluster volume start gluster_vol

Master dan Client
------------------------
#nano /etc/hosts
Tambahkan :
10.2.15.209 master.gluster.local    master
10.2.15.57   client1.gluster.local    client
10.2.15.58   client2.gluster.local    client

Client
--------
#apt update && apt upgrade -y
#apt install software-properties-common -y
#apt install apt-utils
#apt install apt-transport-https
#apt install glusterfs-client -y
#mkdir /mnt/volume1
#mount -t glusterfs client1.gluster.local:/gluster_vol /mnt/volume1
