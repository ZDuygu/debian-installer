# Dhcp-Tftp Server Kurulumu
## Statik IP Atamama
/etc/network/interfaces
```
auto enp0s3
iface enp0s3 inet static
	address 192.168.56.50
	netmask 255.255.255.0
	gateway 192.168.56.1
	broadcast 192.168.56.255
```
## Dhcp konfigürsayon
/etc/dhcp/dhcpd.conf
```
ddns-update-style none;

option domain-name "pardus.lab";
option domain-name-servers linuxdhcp.pardus.lab;

default-lease-time 600;
max-lease-time 7200;

authoritative;

log-facility local7;

subnet 192.168.56.0 netmask 255.255.255.0 {
  range dynamic-bootp 192.168.56.70 192.168.56.80;
  option subnet-mask 255.255.255.0;
  option routers 192.168.56.1;
  allow booting;
  allow bootp;
  filename "pxelinux.0";
  next-server 192.168.56.50;
}
```
## Debian-installer
  Preseed dosyasını debian-installer menusune eklemek için menüye aşağıdaki satırları eklemek gerekir
```
label preseed
        menu label ^Preseed install
        kenrel debian-installer/amd64/linux
        append auto=true locale=tr_TR console-setup/charmap=UTF-8 console-keymaps-at/keymap=tr console-setup/layoutcode=tr console-setup/ask_detect=false pkgsel/language-pack-patterns=pkgsel/install-language-support=false interface=enp0s3 hostname=hostname01 domain="" url=tftp://192.168.56.50/stretch.cfg DEBCONF_DEBUG=5 initrd=debian-installer/amd64/initrd.gz ---quiet
```
# Netboot Linkleri
* [Debian Netboot](http://ftp.nl.debian.org/debian/dists/stretch/main/installer-amd64/current/images/netboot/netboot.tar.gz) - Stretch
* [Pardus Netboot](http://depo.pardus.org.tr/pardus/dists/onyedi/main/installer-amd64/current/images/netboot/netboot.tar.gz) - Onyedi Main
# Kaynaklar
* https://www.debian.org/releases/stable/amd64/index.html.en
* https://www.debian.org/releases/stable/i386/apbs01.html.en#preseed-methods
