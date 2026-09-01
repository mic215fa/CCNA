並將 review 的內容寫入 06_review 這個目錄內# UTP 與 Fiber 決策地圖

tags: #map #decision-map #acting-ccna #cabling

## Sources

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]

## Decision Flow

```text
Need to connect devices
  ↓
Are they typical end hosts to switch on same floor / office?
  ├─ Yes → [[UTP Cable]]
  └─ No
      ↓
Is distance greater than practical UTP range / between floors or buildings?
  ├─ Yes → [[Fiber Optic Cable]]
  └─ No
      ↓
Check cost and device port support
```

## Main Tradeoffs

| Factor | [[UTP Cable]] | [[Fiber Optic Cable]] |
|---|---|---|
| Cost | Lower | Higher |
| Typical use | End host ↔ Switch | Network infrastructure ↔ Network infrastructure |
| Distance | Common Ethernet standards listed at 100 m max | Much longer; depends on MMF/SMF |
| Signal | Electrical over copper | Light through glass fiber |
| EMI | Vulnerable | Not described as vulnerable in this Unit |
| Device support | Most client devices support UTP | Many client devices lack SFP ports |

## Concept Relationships

- [[Fiber Optic Cable]] solves the distance limitation of [[UTP Cable]].
- [[UTP Cable]] remains common because cost and device support matter.
- [[SFP Transceiver]] increases Fiber cost.
- [[MMF and SMF]] refine Fiber decisions by distance and transmitter type.

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]]
