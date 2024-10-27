# mitmrouter
Bash script to automate setup of Linux router useful for IoT device traffic analysis and SSL mitm

```mermaid
flowchart LR
    iot1["Wifi IoT Device"]:::iotDevice
    iot2["Wired IoT Device"]:::iotDevice
    wifi["Wi-Fi Interface(hostapd)"]
    eth["Ethernet Interface"]
    br0["br0"]
    dnsmasq["dnsmasq(dhcp server)"]
    iptables["iptables rules"]
    mitm["mitmproxy(ssl proxy tool)"]
    wan["wan"]
    ethif["Ethernet Interface"]
    
    subgraph Linux_Router[MITM Router]
        style Linux_Router fill:#005500
        br0
        dnsmasq
        iptables
        mitm:::mitm
        wan
    end
    
    iot1 <--> wifi
    iot2 <--> eth
    wifi <---> br0
    eth <---> br0
    br0 <---> dnsmasq
    br0 <--> iptables
    iptables <--> wan
    iptables --> mitm
    wan <--> ethif

    classDef iotDevice fill:#9999ff
    classDef mitm fill:#ffdddd,color:#000
```


## Dependancies

- hostapd
- dnsmasq
- bridge-utils
    - provides `brctl`
- net-tools
    - provides `ifconfig`

## Usage

You may want to disable NetworkManager as it may fight for control of one or more of the network interfaces.

Before running the script, you will need to edit the bash variables at the top of the script to use your machines interface names and other details you may want to change like the Wi-Fi network SSID and password.

```
./mitmrouter.sh: <up/down>
```

The `./mitmrouter.sh up` command will bring down all the linux router components and then build them back up again

The `./mitmrouter.sh down` command will bring down all the linux router components


