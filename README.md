# Placing a Bus-Sized Data Center in Orbit: A Step-by-Step Engineering Walkthrough

This post works through what it would actually take to launch and operate a
648-GPU compute cluster — packaged into something roughly the size of a
40-foot shipping container — in low Earth orbit. The numbers below come from
a rack-and-power budget built earlier in this analysis: nine NVIDIA GB 200
NVL 72 racks (72 GPUs each), consuming roughly 1.08 MW of compute power plus
30–45 kW for the optical scale-out fabric, for a baseline draw around
1.11–1.13 MW before facility overhead.

Real precedent for this idea exists, but at far smaller scale. Axiom Space,
Kepler, and Skyloom have been developing an orbital data center demonstrator
(ODC-T 1) focused on in-space processing for satellites, not GPU training
clusters [2]. Starcloud (formerly known under a different name) has raised
capital specifically to build GPU-equipped space data centers and has
publicly discussed radiative cooling as a selling point [4]. Neither has
flown anything close to a 648-GPU, megawatt-class system — so treat this
walkthrough as a design exercise grounded in physics and hardware specs,
not a description of something that has been built.

## 1. Define the System Envelope

- **Enclosure:** modified 40-ft high-cube container form factor (~12.0 m ×
  2.35 m × 2.69 m internal), pressurized or unpressurized depending on
  cooling architecture.

- **Compute payload:** 9× GB 200 NVL 72 racks, 648 Blackwell GPUs total.

- **Power draw (compute + network):** ~1.11–1.13 MW.

- **Design margin:** add ~15–20% for power conversion losses, pumps,
  avionics, and attitude control, giving a bus-level electrical demand
  around 1.3–1.35 MW.

Flag: GB 200 NVL 72 rack dimensions (roughly 600 mm × 1200 mm footprint,
~2.2 m tall) fit inside the container's height with little clearance to
spare — this is a tight packaging assumption, not a confirmed engineering
fit, and doesn't yet account for structural bracing or the liquid-cooling
manifolds each rack requires.

## 2. Structural Adaptation for Launch Loads

A shipping container is built for static stacking loads, not the vibration
and g-loads of launch. Every rack would need to be re-mounted on a
launch-rated structural frame isolated from the outer shell, with the
liquid-cooling loops, NVLink cabling, and power distribution units secured
against the specific load spectrum of whichever launch vehicle is chosen.
This is a full mechanical redesign, not a retrofit of an existing
container — "bus-sized" describes the volume, not the actual structure that
would fly.

## 3. Mass Budget and Launch Vehicle Selection

Public per-rack mass figures for GB 200 NVL 72 are not something I can verify
precisely, but figures in the range of 1.2–1.4 tonnes per rack have been
cited in industry coverage. Nine racks alone would be roughly 11–13 tonnes.
Add radiator panels, solar arrays, structural frame, thermal loops, power
electronics, and attitude-control hardware, and total launch mass plausibly
lands somewhere in the 25–40 tonne range — a wide-enough estimate that it
should be treated as a placeholder, not a spec.

That mass range sits near or above the LEO payload capacity of a Falcon
Heavy (~26 t) and comfortably within Starship-class capacity (100+ t), so
vehicle choice meaningfully constrains the design rather than being a
formality.

## 4. Orbit Selection

A sun-synchronous LEO orbit (roughly 500–800 km altitude) is the
conventional choice for solar-powered spacecraft: it gives consistent
sun exposure with limited eclipse periods and predictable thermal cycling.
Higher orbits reduce atmospheric drag on large deployed radiator and solar
panels but increase radiation exposure and launch cost. There's a genuine
tradeoff here that a real mission would size numerically, not assume.

## 5. Power Generation

At roughly 300 W/m² of usable solar array output after degradation and
angle losses, meeting a 1.3 MW demand requires on the order of 4,300–4,500
m² of solar array — deployed panels many times larger than the container
itself. That's before accounting for eclipse periods, where battery storage
would need to cover the gap, adding mass and complexity not reflected in
the number above.

## 6. Thermal Rejection — and a Correction Worth Making Explicit

It's worth being precise about *why* radiative cooling works well in space,
because the intuitive framing ("space is cold, so heat radiates away
faster") is not quite right. The relevant physics is the Stefan–Boltzmann
law:

\[
Q = \varepsilon \sigma A T^4
\]

where \(T\) is the temperature of the *radiator surface itself*, not the
ambient temperature of space. The cosmic microwave background sink is
about 2.7 K, which is close enough to zero that it contributes almost
nothing to the equation either way. What space actually gives you is the
absence of air — no convection, no conduction losses — so *all* your waste
heat has to leave via radiation, and the radiator's own emissivity, area,
and operating temperature are what determine how much heat you can reject.

Running the numbers for ~1.1 MW of waste heat, with emissivity \(\varepsilon
\approx 0.9\) and a radiator operating around 300 K:

- One-sided flux: \(0.9 \times 5.67\times10^{-8} \times 300^4 \approx 413\)
  W/m².

- Double-sided panel (radiating from both faces): roughly 826 W/m² of
  panel area.

- Required area: \(1{,}100{,}000 / 826 \approx 1{,}330\) m².

Running the radiator hotter (say 350 K instead of 300 K) roughly doubles
the flux per m² thanks to the \(T^4\) term, cutting required area by close
to half — at the cost of running electronics closer to their thermal
limits and needing a more capable heat-transport loop between the GPUs and
the radiator surface. That's the actual engineering lever, not "space is
cold."

Either way, a radiator array in the 700–1,400 m² range, deployed as
fold-out panels, is a structure far larger than the compute container it's
cooling — a detail that should reshape how "bus-sized" this system really
is once deployed.

## 7. Deployment Sequence

1. **Ground integration and testing** — full thermal-vacuum chamber
   testing of the assembled cluster before launch; this is the only chance
   to catch problems, since on-orbit repair of commodity GPU hardware is
   not realistically available.
2. **Launch and ascent** — payload fairing must accommodate the container
   plus folded solar/radiator panels.
3. **Orbit insertion and circularization.**
4. **Deployment**: solar arrays unfold first (power is needed before
   anything else can run), followed by radiator panel deployment, followed
   by structural locking of both.
5. **Cold start and commissioning**: power up racks sequentially, bring up
   NVLink domains within each rack, then bring up the optical scale-out
   fabric across racks.
6. **Attitude lock**: orient solar arrays sun-facing and radiator panels
   toward deep space, away from Earth's infrared and reflected sunlight,
   which otherwise degrades radiator performance.
7. **Communications link establishment** — likely optical inter-satellite
   or ground links, given the data volumes involved; RF alone would be a
   bottleneck at this scale.
8. **Steady-state operations and monitoring.**
9. **End-of-life plan** — deorbit or graveyard-orbit disposal, required by
   most current orbital debris mitigation guidelines.

## 8. Open Risks Worth Naming Plainly

- **Radiation effects on GPUs**: consumer/datacenter-class silicon is not
  rad-hardened; expect elevated bit-error rates from cosmic ray strikes,
  requiring aggressive ECC and possibly shielding mass not included above.

- **No serviceability**: a failed GPU or rack in orbit is likely
  unrecoverable — there's no equivalent of swapping a failed unit on the
  ground.

- **Debris and micrometeorite risk** to large, thin deployed radiator
  panels, which are more vulnerable than a compact spacecraft bus.

- **Economic case**: current real-world orbital data center efforts are
  early demonstrators focused on niche use cases like reducing downlink
  latency for other satellites [2][4][6], not general-purpose AI training
  at hundreds-of-kilowatts-to-megawatt scale. The launch, radiator, and
  solar array mass required to support 648 GPUs is substantial relative to
  the compute payload itself — worth weighing against simply building more
  efficient terrestrial data centers before assuming space is the cheaper
  path.

## Summary Numbers

| Parameter | Estimate |
|---|---|
| GPUs | 648 (9× GB 200 NVL 72 racks) |
| Compute + network power | ~1.11–1.13 MW |
| Design electrical budget (with overhead) | ~1.3–1.35 MW |
| Solar array area | ~4,300–4,500 m² |
| Radiator area (300 K operating temp) | ~1,300–1,400 m² |
| Estimated launch mass | ~25–40 t (wide, unverified range) |

All figures above are order-of-magnitude engineering estimates built from
published hardware specs and physics, not a validated mission design —
several of the inputs (rack mass, radiator emissivity in practice, array
degradation rates) are flagged above as approximate and should be checked
against vendor datasheets or a real thermal/power analysis before being
treated as authoritative.
