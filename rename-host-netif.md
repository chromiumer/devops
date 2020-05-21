 
1.rename config file.
```
mv /etc/sysconfig/networks/ifcfg-enp1s0 /etc/sysconfig/networks/ifcfg-eth0
```

2.add params.
```
vim /etc/sysconfig/grub

...
GRUB_CMDLINE_LINUX="xxx rhgb quiet net.ifnames=0 biosdevname=0"
...

##add 'net.ifnames=0 biosdevname=0' after 'xxx rhgb quiet'.
```

3.apply config params
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
4.reboot host
```
reboot
```
