# Orbit Decision: Dawn-Dusk Sun-Synchronous Orbit (SSO)

## Decision
Baseline the cluster to a **dawn-dusk sun-synchronous orbit**, typically
~600–800 km altitude, ~97–98° inclination (retrograde, near-polar).

## What this orbit does
A sun-synchronous orbit precesses at the same rate the Earth orbits the
sun, so the orbital plane maintains a fixed relationship to the sun. The
dawn-dusk sub-type places the orbital plane along the terminator (the
day/night boundary on Earth), which means the spacecraft rides almost
continuously in daylight — it never passes through Earth's shadow except
during brief windows near the equinoxes.

## Rationale

1. **Eliminates most of the eclipse penalty that drives array oversizing.**
   A standard LEO orbit spends roughly 35–40% of each period in shadow,
   which forced the ~1.65× array oversizing margin and a battery bank sized
   to carry ~1.3 MW through every eclipse, every orbit, for years. Dawn-dusk
   SSO removes nearly all of that recurring eclipse, collapsing the solar
   array requirement to the lower "continuous-sun" estimate — roughly
   2,150–2,250 panels rather than 3,550–3,750.

2. **Removes the dominant driver of battery mass.**
   Batteries sized to bridge a 35% eclipse fraction at megawatt-class load
   are not a minor subsystem — plausibly comparable in mass to the solar
   array itself. Cutting the eclipse burden to a few days per year near
   equinox turns the battery bank from a primary structural item into a
   much smaller buffer for transients and faults.

3. **Simplifies attitude control for both arrays and radiators.**
   With the sun direction effectively fixed relative to the orbital plane,
   the array can hold a near-constant sun-pointing attitude without
   aggressive slewing, and the radiator face can be kept consistently
   oriented away from both the sun and Earth's infrared flux — directly
   supporting the attitude-lock step (Step 6) and the radiator-sizing
   assumptions in the thermal budget.

4. **Predictable, well-characterized orbit class.**
   Dawn-dusk SSO is a mature, frequently flown orbit for
   power-hungry/imaging spacecraft, so its perturbation behavior,
   station-keeping needs, and thermal environment are well understood —
   this is a lower-risk choice than an exotic or bespoke orbit.

## Costs and open risks of this choice

- **Not zero-eclipse.** Near the equinoxes, dawn-dusk SSO does pass through
  brief shadow periods (on the order of minutes per day, for a window of
  days). The battery bank still needs sizing for that, not zero.

- **Inclination requirement affects launch cost.** ~97–98° retrograde
  inclination is not a free launch — it typically costs more delta-v than
  an equatorial or low-inclination orbit, which is a real number this
  document has not scoped.

- **Radiation and atomic oxygen exposure differ from other LEO choices**
  at this altitude band and inclination, feeding back into the array
  degradation margin used in the power budget — worth revisiting with a
  mission-specific radiation model, not the generic multi-year degradation
  assumption used earlier.

- **Orbit-keeping propellant/station-keeping** is an ongoing mass and
  operations cost not yet included in any budget in this design.

## Bottom line
This orbit choice is being made specifically to protect the power and
thermal budgets already built, not because it's cost-free. The launch and
station-keeping penalties are real and unquantified here — they'd need a
mission-design pass before this is anything more than a directionally
sound assumption.
