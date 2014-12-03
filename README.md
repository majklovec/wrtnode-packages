wrtnode-packages
================
```
echo "src-git node git://github.com/majklovec/wrtnode-packages.git" >> ./feeds.conf
scripts/feeds/update -a
scripts/feeds/install -p node -a
make menuconfig
make -j13
```
