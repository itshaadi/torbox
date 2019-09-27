<p align="center">
  <h3 align="center">Tor Box</h3>
  <p align="center">Container-based Tor access point.</p>

  <p align="center">
    <a href="https://github.com/GouveaHeitor/nipe/blob/master/LICENSE.md">
      <img src="https://img.shields.io/badge/license-MIT-blue.svg">
    </a>
  </p>
</p>

---

<p align="center">
    <img src="https://user-images.githubusercontent.com/10201704/65461099-245fde00-de60-11e9-95e4-1f0c57806c3f.jpg" alt="how it works">
</p>

### How it works
    Tor Box is a script for Setting up a Tor wireless access point using docker containers.
    it's aimed to be used as an Always-On anti-censorship BACKUP gateway.

    This script enables you to directly route traffic of connected devices to the Tor network
    Currently Tor Box only supports IPv4, and only traffic from TCP/DNS/DHCP is allowed
    any non-local traffic from UDP/ICMP is blocked.

    this script is not optimized for visiting hidden services. please use Tor Browser instead.
    Tor Box is to be used only as a BACKUP for situations where popular solutions (such as OpenVPN)
    are temporarily blocked by a state firewall.

    This goal is achived by sharing a Network Namespace between the containers.


#### Requirements:
* wireless network card/adapter (with proper drivers already installed).
* following packages must be installed: `iw`, `iptables`, `docker`

#### Defaults:
* Gateway: `192.168.162.1/24`
* SSID: `Tor Box`
* Passphrase: `torbox12345`

**note:** edit `templates/torrc.template` according to your needs but do not touch `TransPort` and `DNSPort`

### Installation
Tested on:
* Debian 10 with RT5370 Wireless Adapter
* Raspberry Pi 3

```
git clone https://github.com/itshaadi/torbox.git

cd torbox

chmod +x torbox
```

useful documentations:
* [hostapd](https://wiki.gentoo.org/wiki/Hostapd)
* [dnsmasq](https://wiki.archlinux.org/index.php/Dnsmasq)

### Usage

```
./torbox help

Usage: 
 	 <start|stop> <interface> 
 	 <log> <container>

eg: ./torbox start wlan0
    ./torbox log torbox-tor
    ./torbox stop wlan0
```

#### nmap results
```
sudo nmap -sU -p 10558 38.84.132.167 # us1.freeopenvpn.org

Starting Nmap 7.80 ( https://nmap.org ) at XXXXXX
Nmap scan report for 38.84.132.167
Host is up (0.00085s latency).

PORT      STATE    SERVICE
10558/udp filtered unknown

sudo nmap -sU -p 9061 192.168.162.1 # local UDP is allowed

Starting Nmap 7.80 ( https://nmap.org ) at XXXXX
Nmap scan report for 192.168.162.1
Host is up (0.00053s latency).

PORT     STATE  SERVICE
9061/udp open   unknown
MAC Address: XXXXXXXX (Tenda Technology)
```
>  Filtered means that a firewall, filter, or other network obstacle is blocking the port so that Nmap cannot tell whether it is open or closed. [source](https://wiki.onap.org/display/DW/Nmap)
