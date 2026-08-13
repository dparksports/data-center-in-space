# Hypothetical Rubin NVL 72 Orbital Power Unit — Structure & Deployment Options

> **Caveat up front:** every number below is an extrapolation, not a confirmed NVIDIA or launch-provider spec. NVIDIA has not published a rack-level power figure for a 72-GPU Rubin configuration; the 250 kW figure carried over from the prior message is the more defensible of two mismatched estimates (rack-level SuperPOD data vs. bare chip TDP), not a validated design number. Treat this whole document as a planning sketch, not an engineering baseline.

## 1. Rack power budget (assumed)

| Item | Value | Confidence |
|---|---|---|
| GPU count | 72 (Rubin) | Assumed, matches prior GB 200 NVL 72 headcount |
| Rack power draw | ~250 kW | Extrapolated from DGX SuperPOD Rubin NVL 8 rack data (~225 kW / 64 GPUs), scaled — NOT a published NVL 72 spec |
| Implied per-GPU power | ~3.5 kW | Rack-effective, includes CPUs/switches/cooling overhead |
| Continuous annual energy | ~2,190 MWh/yr | 250 kW × 8,760 h; ignores eclipse gaps, degradation, margin |

## 2. Solar structure options to cover 250 kW

Baseline unit: one ISS-class solar array wing set (~120 kW published generation capacity, end-of-life, ~2,500 m² active area) [2]. Two structural approaches to close the ~250 kW requirement in a single launch:

| Option | Configuration | Combined output | Gap vs. 250 kW load | Stowed footprint (rough) |
|---|---|---|---|---|
| A — Single larger structure | One array scaled to ~2.1x ISS wing area (~5,200 m²) | ~250–260 kW (extrapolated linearly from ISS output/area — solar structures don't scale perfectly linearly with area due to mast stiffness and deployment mechanics, so treat as optimistic) | Roughly closed, if linear scaling holds | Larger single stowed module; folded mast length becomes the binding launch constraint |
| B — Binary (two ISS-class arrays) | Two independent ISS-wing-class arrays, each ~120 kW | ~240 kW combined | **Short by ~10 kW** — does not fully close the budget even before eclipse/degradation losses | Two separate folded units; more failure-tolerant (one array failure ≠ total loss) but doesn't reach the target on its own |

Neither option accounts for: eclipse time (any non-dawn/dusk sun-synchronous orbit loses a third of each ~90-minute cycle to shadow, requiring battery buffering or further oversizing), angle-of-incidence losses as orientation drifts, or on-orbit degradation from radiation and micrometeoroid damage — all of which push the real requirement upward, not downward. Option B in particular is already short on paper before any of those real-world losses are applied.

## 3. Deployment method

| Method | Status | Assessment |
|---|---|---|
| Astronaut EVA assembly | Real, used for ISS's actual arrays | Slowest, most expensive, requires crewed vehicle + life support; not viable as a repeatable model |
| **Tesla robotic assembly (requested)** | **Not currently viable** | As of early 2026, Tesla's own public statements indicate Optimus is not yet performing useful work even in a controlled factory environment [per prior turn's sourcing] — a far easier setting than unstructured assembly in vacuum, microgravity, and thermal extremes with no atmosphere for cooling. There is no evidence this platform, or any humanoid robot, is being adapted for orbital work. Including it here as requested, but it should be treated as speculative placeholder, not a real deployment path. |
| Autonomous/self-deploying structure | Standard industry practice today | Spring-loaded or motor-driven boom deployment, the same mechanism used by essentially every commercial satellite and by ISS's own array *blankets* (folded accordion-style, motor-unfurled). No human or robot intervention needed after release from the launch vehicle. This is the realistic method for either Option A or B. |

## 4. Bottom line

The single-launch constraint is a packaging and mass problem more than a power-generation one — Option A (one larger structure) is the cleaner fit for a single launch and comes closer to matching the 250 kW load, provided the linear area-to-power scaling assumption holds up under real engineering review, which it may not. Option B (binary arrays) is inherently redundant but falls short of the budget even in the optimistic case. The "Tesla robotic assembly" placement method, as requested, does not currently exist as a viable capability and should not be relied on for a real deployment plan — autonomous self-deployment is the only method with actual flight history behind it.
