# Ethernet Switching 與 ARP 地圖

tags: #map #acting-ccna #ethernet #switching #arp

## Scope

- Source Note：[[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- Source：[[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]]
- Core concepts：[[Ethernet Switching]]、[[MAC Address Learning]]、[[Frame Forwarding]]、[[Address Resolution Protocol]]

## Concept Structure

```mermaid
flowchart TD
  E["Ethernet Switching"] --> L["MAC Address Learning"]
  L --> T["MAC Address Table"]
  T --> A["Dynamic entry aging"]
  E --> F["Frame Forwarding"]
  F --> K["Known unicast"]
  F --> U["Unknown unicast"]
  U --> FL["Frame Flooding"]
  F --> B["Broadcast"]
  B --> FL
  ARP["ARP"] --> B
  ARP --> C["ARP cache"]
```

## Switching Decision Flow

```mermaid
flowchart TD
  A["Switch receives Ethernet Frame"] --> B["Learn source MAC on ingress port"]
  B --> C{"Destination MAC in MAC Address Table?"}
  C -->|"Yes, other port"| D["Known Unicast: Forward out mapped port"]
  C -->|"Yes, ingress port"| H["Filter: do not send back"]
  C -->|"No"| E["Unknown Unicast: Flood out all other ports"]
  A --> F{"Broadcast destination?"}
  F -->|"ffff.ffff.ffff"| G["Flood within broadcast domain"]
```

## ARP Flow

```mermaid
flowchart TD
  A["Host knows destination IPv4 address"] --> X{"Destination in local subnet?"}
  X -->|"Yes"| Y["Next-hop IP = destination host"]
  X -->|"No"| Z["Next-hop IP = default gateway"]
  Y --> B{"Next-hop mapping cached?"}
  Z --> B
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
- [[Frame Forwarding#Unknown Unicast Decision|Unknown Unicast]] → causes → [[Frame Flooding]]
- [[Broadcast Frame]] → causes → [[Frame Flooding]]
- [[Address Resolution Protocol]] → uses broadcast first, then unicast reply
- [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP Table]] → reduces repeated ARP requests
- [[MAC Address Table#Dynamic Entries and Aging|MAC Aging]] → removes stale Layer 2 forwarding state
- ARP cache and [[MAC Address Table]] → are complementary, not interchangeable
- Remote-subnet destination → causes host to ARP for → [[Default Gateway]], not the remote host

## State Tables Compared

| Table | Owner | Key → value | Question answered |
|---|---|---|---|
| [[MAC Address Table]] | Switch | MAC + VLAN → port | 這個 frame 應從哪個 port 送出？ |
| [[Address Resolution Protocol#ARP Cache (ARP Table)|ARP cache]] | Host／router | IPv4 → MAC | 要建立 next-hop frame 時應使用哪個 MAC？ |

## Troubleshooting Flow

```mermaid
flowchart TD
  A["IP communication fails"] --> B{"ARP mapping exists?"}
  B -->|"No"| C["Check target/next-hop IP and broadcast path"]
  B -->|"Yes"| D{"MAC learned in correct VLAN/port?"}
  D -->|"No"| E["Check source-MAC learning and Layer 2 path"]
  D -->|"Yes"| F{"Destination known and egress forwarding?"}
  F -->|"No"| G["Check port/VLAN/STP state"]
  F -->|"Yes"| H["Continue with Layer 3 and endpoint checks"]
```

## Core Boundaries

- ARP failure：sender 無法取得建立 next-hop Ethernet frame 所需的 MAC。
- MAC-learning failure：switch 無法建立可靠的 MAC/VLAN/port state。
- Forwarding failure：state 存在，但 egress selection 或 port/VLAN state 阻止 frame 通過。
- Flooding：可能是正常 discovery/fallback，也可能因 learning instability 而持續發生。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-102_700_1269_1225_238.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.3 — Switches build MAC address tables by learning source MAC addresses.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-104_717_1402_181_236.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.4 — Unknown unicast flooding when MAC address tables are empty.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-108_698_1401_183_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.6 — ARP request and reply exchange between PC1 and PC3.*

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit03-CLI-Eth-IPV4 REVIEW|Unit03-CLI-Eth-IPV4 REVIEW]]
