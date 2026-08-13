# Electricity Budget — 9× GB 200 NVL 72 Racks (648 GPUs)

Power figures use NVIDIA's published GB 200 NVL 72 rack rating (~120 kW/rack)
and industry-typical optical transceiver/switch draw. Switch counts and
per-unit wattages are estimates, not vendor-confirmed spec-sheet figures.

| Component | Power Draw | Notes |
|---|---|---|
| Compute racks (9× GB 200 NVL 72 @ ~120 kW) | ~1,080 kW | 648 Blackwell GPUs total |
| Optical transceivers (1,296 × ~12 W) | ~15.6 kW | 1 per GPU NIC + 1 per switch port |
| Leaf switches (~11 units) | ~14 kW | Quantum-2 class, ~1.3 kW each |
| Spine switches (fat-tree topology) | ~14 kW | Non-blocking inter-rack fabric |
| *Optical scale-out layer subtotal* | *~30–45 kW* | ~3–4% of compute load |
| **Compute + network subtotal** | **~1,110–1,125 kW** | GPU + optical fabric only |
| Facility overhead (power conversion, pumps, avionics, ~15–20%) | ~165–225 kW | Space-based equivalent of terrestrial PUE losses |
| **Total design electrical budget** | **~1,300–1,350 kW** | All-in continuous draw |
| Daily energy use | ~31.2–32.4 MWh/day | At full 100% capacity, 24×7 |
| Annual energy use | ~11.4–11.8 GWh/year | Comparable to ~1,000–1,100 avg. U.S. households/year |

**Caveat:** the per-switch, per-transceiver, and overhead-percentage figures
are engineering estimates built on top of NVIDIA's published GB 200 NVL 72
rack rating, not numbers independently verified against a current
datasheet — check against NVIDIA's spec sheet before treating this as final.
