Sometimes we need access virtual machine OS’s web page, at the VMware Fusion 8, the default network setting is NAT, how to config it.

>**Update:** VMWare Fusion 8.5.0 fixed the issue for NAT mapping, please upgrade to 8.5.0.

>**Caution:** VMWare Fusion 8.1 had an issue for NAT mapping, please don’t upgrade to 8.1 if you want to use NAT mapping.

1. Set a Static IP for your virtual machine system.

Modify **dhcpd.conf**
```Bash
sudo vim /Library/Preferences/VMware Fusion/vmnet8/dhcpd.conf
```

After where it says **End of “DO NOT MODIFY SECTION”** add the following lines:

```Bash
host Windows8x64 {
    hardware ethernet 00:0C:29:B6:22:3E;
    fixed-address 172.16.106.128;
}
```

**Windows8x64** — Replace it use your virtual machine name

**hardware ethernet address** — use your VMWare Fusion’s virtual MAC address.

>**Important**: You must allocate an IP address that is outside the range defined inside the DO NOT MODIFY SECTION section.

Quit VMWare Fusion, restart it.

2. Change NAT configure file.

```Bash
sudo vi /Library/Preferences/VMware Fusion/vmnet8/nat.conf
```

find **[incomingtcp]** part, like this

```Bash
[incomingtcp]
# Use these with care — anyone can enter into your VM through these…
# The format and example are as follows:
#<external port number> = <VM’s IP address>:<VM’s port number>
#8080 = 172.16.3.128:80
```

Add your configure, for example:

```Bash
[incomingtcp]
# Use these with care — anyone can enter into your VM through these…
# The format and example are as follows:
#<external port number> = <VM’s IP address>:<VM’s port number>
#8080 = 172.16.3.128:80
80 = 172.16.106.128:80
```

It means we map virtual machine 80 port to host machine 80 port.

3. Restart network service of VMware Fusion.

```Bash
sudo /Applications/VMware Fusion.app/Contents/Library/vmnet-cli --stop
sudo /Applications/VMware Fusion.app/Contents/Library/vmnet-cli --start
```
>**Note**: The config files you changed will be reset after VMWare Fusion upgrade, please backup it at some where.

That’s all.
