# Network Configuration

> [!important]
> Please don't re-configure this router without coordinating with Lasse or Thomas.

## Credentials

```
wifi-pass: <shared with you upon lab-saftey training; do not share with others>
ssid: mrl-wifi
ssid-5g: mrl-wifi-5g
router-ip: 192.168.0.1
```

## IP Addressing

> [!important]
> The router software does not resolve hostnames (no local DNS). If you need that feature (e.g. for remote control of the Jackal), edit your `/etc/hosts` file accordingly.

- The router is running a DHCP server. Hence, when you connect, **you will get an IP address dynamically assigned**. This is the default behavior for most devices.
- The DHCP server is configured to assign  **fixed IPs to robots and devices that permanently reside in the lab space**. Robots should connect via DHCP and *not* locally configure their IP. All IP configuration is only in the router.

The following devices have permanent IPs in the DHCP server of the lab router:

| Device (Hostname if different) | Interface     | IP                |
| -------------------            | ------------- | ----------------- |
| Router                         | Ethernet      | `192.168.0.1`     |
| Vicon-PC                       | Ethernet      | `192.168.0.232`   |
| jackal-laptop                  | Wifi          | `102.168.0.200`   |
| Jackal 01                      | WIFI          | `192.168.0.101`   |
| Jackal 02                      | WIFI          | `192.168.0.102`   |
| Jackal 03 (cpr-j100-0594)      | WIFI          | `192.168.0.103`   |
| Jackal 04                      | WIFI          | `192.168.0.104`   |
| Dingo 1 (cpr-do100-duot13)     | WIFI          | `192.168.0.121`   |
| Dingo 2 (cpr-do100-10000062)   | WIFI          | `192.168.0.122`   |
| JetRacer 1                     | WIFI          | `192.168.0.131`   |
| JetRacer 2                     | WIFI          | `192.168.0.132`   |
| JetRacer 3                     | WIFI          | `192.168.0.133`   |
| JetRacer 4                     | WIFI          | `192.168.0.134`   |
| Falcon 1                       | WIFI          | `192.168.0.141`   |
| Falcon 2                       | WIFI          | `192.168.0.142`   |
| Falcon 3                       | WIFI          | `192.168.0.143`   |
| Falcon 4                       | WIFI          | `192.168.0.144`   |
| Falcon 6                       | WIFI          | `192.168.0.146`   |
| Falcon 7                       | WIFI          | `192.168.0.147`   |
| Hovergames  Drone 1            | WIFI          | `192.168.0.151`   |
| Hovergames  Drone 2            | WIFI          | `192.168.0.152`   |
| Mantis Drone 1          | WIFI          | `192.168.0.157`   |
| Spot                           | WIFI          | `192.168.2.11`    |

## VLAN

> [!note]
> This is primarily an internal note for the admins and resides here to increase the truck factor.

The mobile robotics lab has its own VLAN. The VLAN can be configured only by the admins.
Currently, admins include:
- Thomas
- André
- Lasse

If in the future someone else needs to have admin permissions, you have to request it from ICT by making a service request via one of the existing admins.

Once you have admin permissions, you can configure the VLAN at <https://infra-ict.tudelft.nl/>.
