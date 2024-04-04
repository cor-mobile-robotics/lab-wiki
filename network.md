
# Mobile Robotics Lab Network

## TP-Link Router

### Credentials

```
wifi-pass: (check bottom of router)
ssid: TP-Link_5F80_5G
router-ip: 192.168.0.1
```

### Static IP Bindings

The following robots have a statically bound IP in the router:

| Robot                                       | Interface     | IP                |
| ------------------------------------------- | ------------- | ----------------- |
| [Jackal 01](../../robots/jackal#Jackal01)   | WIFI          | `192.168.0.101`   |
| [Jackal 02](../../robots/jackal#Jackal02)   | WIFI          | `192.168.0.102`   |
| [Jackal 03](../../robots/jackal#Jackal03)   | WIFI          | `192.168.0.103`   |
| [Jackal 04](../../robots/jackal#Jackal04)   | WIFI          | `192.168.0.104`   |
| [Dingo 1](../../robots/dinova/dingo.md#Dingo1)   | WIFI          | `192.168.0.121`   |
| [Dingo 2](../../robots/dinova/dingo.md#Dingo2)   | WIFI          | `192.168.0.122`   |

# Miscellaneous Technical Notes

- Please don't re-configure this router without coordinating with @lassepe or Thijs Niesten.
- The router software does not resolve hostnames (no local DNS). If you need that feature (e.g. for remote control of the Jackal), edit your `/etc/hosts` file accordingly.

## VLAN

The mobile robotics lab has its own VLAN. The VLAN can be configured only by the admins.
Currently, admins include:
- Thomas
- André
- Lasse

If in the future someone else needs to have admin permissions, you have to request it from ICT by making a service request via one of the existing admins.

Once you have admin permissions, you can configure the VLAN at <https://infra-ict.tudelft.nl/>.
