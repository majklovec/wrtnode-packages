wrtnode-packages
================
Easy instalation with extroot. Prepare your USB (ext2, or other filesystem, just change the parameter of mount)

```
cat <<EOF > /etc/config/fstab
config global 'automount'
        option from_fstab '1'
        option anon_mount '1'

config global 'autoswap'
        option from_fstab '1'
        option anon_swap '0'

config mount
        option device '/dev/sda1'
        option options 'rw,sync,noatime'
        option enabled_fsck '0'
        option enabled '1'
        option target '/overlay'
        option fstype 'ext2'

config swap
        option device '/swapfile'
        option enabled '1'
EOF

/etc/init.d/fstab enable

cat <<EOF > /etc/opkg.conf
src/gz barrier_breaker http://d.wrtnode.com/packages
src/gz node http://wrtnode.3draty.cz
dest root /
dest ram /tmp
lists_dir ext /var/opkg-lists
option overlay_root /overlay
EOF

mkdir -p /mnt/sda1
mount /dev/sda1 /mnt/sda1 -t ext2 -o rw,sync,noatime

tar -C /overlay -cvf - . | tar -C /mnt/sda1 -xf -

dd if=/dev/zero of=/mnt/sda1/swapfile bs=1024 count=65536
mkswap /mnt/sda1/swapfile

reboot

opkg update
opkg install node-serialport node-socket.io
```


From Source
```
echo "src-git node git://github.com/majklovec/wrtnode-packages.git" >> ./feeds.conf
scripts/feeds/update -a
scripts/feeds/install -p node -a
make menuconfig
make -j13
```
