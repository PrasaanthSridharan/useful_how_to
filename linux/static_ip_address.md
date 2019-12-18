# Configuring Static IP Address with "ip" instead of "ipconfig"

Original source: https://linuxize.com/post/how-to-configure-static-ip-address-on-ubuntu-18-04/#configuring-static-ip-address-on-ubuntu-server

First, run the command `ip link` to determine which interface you will be using to configure your static ip address for. 

The format of you interface should be `en<some letter><some number>`.

You will modify (or create) your configuration file in `/etc/netplan/01-netcfg.yml`. The following is an example of what the file may contain: 

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eno1:
      dhcp4: no
      addresses:
        - 192.168.2.86/24
      gateway4: 192.168.2.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

After saving, run `sudo netplan apply`. Your configuration should now be applied and your device now has a static IP Address.
