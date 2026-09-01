# Ethernet Switching 與 ARP 地圖

tags: #map #acting-ccna #ethernet #switching #arp

## Scope

- Source Note：[[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- Source：[[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]]

## Switching Decision Flow

```mermaid
flowchart TD
  A["Switch receives Ethernet Frame"] --> B["Learn source MAC on ingress port"]
  B --> C{"Destination MAC in MAC Address Table?"}
  C -->|"Yes"| D["Known Unicast: Forward out mapped port"]
  C -->|"No"| E["Unknown Unicast: Flood out all other ports"]
  A --> F{"Broadcast destination?"}
  F -->|"ffff.ffff.ffff"| G["Flood within broadcast domain"]
```

## ARP Flow

```mermaid
flowchart TD
  A["Host knows destination IPv4 address"] --> B{"Knows MAC address?"}
  B -->|"No"| C["Send ARP Request as Broadcast Frame"]
  C --> D["Switches Flood"]
  D --> E["Target host sends ARP Reply as Unicast Frame"]
  E --> F["Sender stores mapping in ARP Table"]
  B -->|"Yes"| G["Encapsulate packet in Ethernet Frame"]
  F --> G
```

## Important Relationships

- [[Ethernet Frame]] → Source field → [[MAC Address Learning]]
- [[MAC Address Learning]] → [[MAC Address Table]]
- [[MAC Address Table]] → determines → [[Frame Forwarding]] vs [[Frame Flooding]]
- [[Unknown Unicast Frame]] → causes → [[Frame Flooding]]
- [[Broadcast Frame]] → causes → [[Frame Flooding]]
- [[Address Resolution Protocol]] → uses broadcast first, then unicast reply
- [[ARP Table]] → reduces repeated ARP requests

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — Switches build MAC address tables by learning source MAC addresses.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-104_717_1402_181_236.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.4 — Unknown unicast flooding when MAC address tables are empty.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-108_698_1401_183_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.6 — ARP request and reply exchange between PC1 and PC3.*

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit03-CLI-Eth-IPV4 REVIEW|Unit03-CLI-Eth-IPV4 REVIEW]]

