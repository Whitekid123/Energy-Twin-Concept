# Deep Research Document: Pure-Software Approaches for Other Energy Forms
## Generators, Mechanical (Pumps/Motors), Small Wind, and Multi-Energy Systems

**Prepared for research supporting pure-software energy tools in Nigeria**  
**Aligned with original conversation goals:** tracking like a budget, calculation tools, pure software (no heavy IoT), multi-form energy, sparse data

---

## 1. What Was Found and Could Be Used (Summary Across All Forms)

### Overall Finding
Pure-software calculation and estimation methods exist for generators, pumps/motors, small wind, and multi-energy combinations. They rely on the same philosophy as the electrical bottom-up work: user-provided sparse inputs (ratings, hours, prices, location estimates) rather than continuous sensors.

**What can be used immediately:**
- Generator fuel-cost and efficiency calculators from kVA + runtime + fuel price
- Pump/motor cost and efficiency-range estimators from size + hours + energy cost
- Small-wind annual yield and payback screening from average wind speed + turbine size
- Simple multi-source ranking tools that treat grid + generator + mechanical loads together

These can all be built as pure software and support the calculation and awareness goals in the original voice note.

---

## 2. Generators – Fuel Consumption, Efficiency and Sizing

### What Was Found
- Standard fuel-curve models are used in hybrid energy software (HOMER Pro and open-source equivalents). Fuel consumption is expressed as litres per hour at different percentages of rated load.
- Nigerian hybrid-system studies repeatedly show that generators lose efficiency when oversized or run at light load (efficiency can drop from ~50% range to ~20% range).
- Simple mathematical relationships link load percentage, fuel rate and electrical output. These can be calculated from manufacturer data or typical values.
- Optimisation research for Nigerian buildings routinely includes diesel/gasoline generators alongside other sources and uses software to find least-cost operating strategies.

### Calculation Logic (Pure Software)
Typical steps:
1. User enters: generator rated kVA (or kW), approximate daily or monthly runtime hours, current fuel price per litre.
2. Software applies a fuel curve (or average specific fuel consumption) to estimate litres used.
3. Cost = litres × price.
4. Cost per kWh = total fuel cost ÷ estimated kWh produced.
5. Optional: show the load band (usually 50–80% of rating) where fuel waste is lowest.

Example conceptual equation used in literature:
```
Fuel rate (L/h) ≈ a × P + b
```
where P is the electrical load (kW), and a, b are coefficients from the fuel curve.

Monthly fuel cost ≈ fuel rate × hours × price.

### What Can Be Used
A pure-software tool that answers:
- “How much is this generator costing me per month?”
- “At what load does it waste the least fuel?”
- “What is the true cost per unit of electricity from the generator right now?”

This directly supports the calculation-tool part of the original conversation and needs no sensors.

---

## 3. Mechanical Energy – Pumps and Motors

### What Was Found
- Pump and motor efficiency can be estimated from nameplate data, operating hours, and basic process information using established mathematical models and affinity laws.
- Software tools (including spreadsheet-based) exist that calculate system efficiency from a small set of inputs: motor size, speed, estimated power, flow/head if known.
- Efficiency maps and polynomial approximations allow prediction of performance across operating points without continuous monitoring.
- Non-intrusive methods using electrical signals exist, but pure calculation approaches that rely only on user-entered hours and ratings are also documented and practical for sparse-data settings.

### Calculation Logic (Pure Software)
Typical steps:
1. User enters: pump/motor rated power or size, estimated daily hours of use, cost of electricity or fuel.
2. Software applies standard efficiency curves or affinity laws to estimate actual energy use.
3. Monthly cost and a flag for “likely running outside efficient range” are produced.

Affinity laws (simplified):
```
Power₂ / Power₁ ≈ (Speed₂ / Speed₁)³
Flow₂ / Flow₁ ≈ Speed₂ / Speed₁
```
These allow scaling estimates when speed or operating point changes.

### What Can Be Used
A pure-software calculator for borehole pumps and small motors that shows:
- Estimated monthly energy cost
- Whether the unit is likely oversized or running inefficiently
- Simple savings if operating hours are reduced or the unit is right-sized

This is highly relevant for Nigerian households and small businesses that rely on water pumps.

---

## 4. Small-Scale Wind Energy

### What Was Found
- Wind resource assessment in Nigeria commonly uses the two-parameter Weibull distribution. Multiple numerical methods estimate the shape (k) and scale (c) parameters from historical wind-speed data.
- The Energy Commission of Nigeria has a Wind Information System (WIS) software for assessing potential. Validation studies compare its outputs with independent calculations.
- Power-curve based annual energy production (AEP) calculations are standard: given a wind-speed distribution and a turbine’s power curve, yearly electricity can be estimated.
- Studies across Nigerian cities provide wind-power density figures and evaluate small turbines for low-wind-speed conditions. Coastal and some inland sites show higher potential.
- Pure calculation methods (public wind data or a simple average wind-speed estimate) are widely used for preliminary screening and do not require on-site sensors for early assessment.

### Calculation Logic (Pure Software)
Typical steps:
1. User enters: location or approximate average wind speed, proposed hub height, turbine rated power or power-curve points.
2. Software applies Weibull distribution (or simplified average) and integrates with the power curve to estimate annual kWh.
3. Simple payback = turbine cost ÷ (annual kWh × electricity value in naira).

Weibull probability density (conceptual):
```
f(v) = (k/c) × (v/c)^(k-1) × exp[−(v/c)^k]
```
Annual energy is the integral of power-curve(v) × f(v) × hours.

### What Can Be Used
A pure-software screening tool that answers:
- “Is a small wind turbine worth considering at this location?”
- “Roughly how much electricity and payback can I expect?”

This helps avoid buying equipment that will never pay back and fits the pure-software, start-small direction.

---

## 5. Multi-Energy / Cross-Form Approaches

### What Was Found
- Hybrid optimisation tools (HOMER and open-source alternatives) routinely combine generators, solar, batteries and sometimes wind. They use load profiles, fuel curves and resource data to find least-cost combinations.
- Open-source and web-based decision-support systems have been developed for developing-country contexts where data and local expertise are limited.
- Simple software prototypes in Nigerian research compute household energy demand from appliance lists and then evaluate mixes of grid + generator + renewables.

### Calculation Logic (Pure Software)
User logs the different sources they actually use (grid hours/prepaid, generator hours/fuel, any mechanical loads). Software ranks combinations by estimated naira cost and highlights the cheapest realistic options under current prices.

### What Can Be Used
A unified pure-software view that treats electricity + generator + mechanical loads as one system and answers practical “what-if” cost questions. This matches the multi-form intent in the original conversation.

---

## 6. Comparison Table Across Forms

| Energy Form       | Core Pure-Software Method              | Minimum User Inputs                     | Research Support Level                  | Main Limitation                          | Immediate Usable Output                     |
|-------------------|----------------------------------------|-----------------------------------------|-----------------------------------------|------------------------------------------|---------------------------------------------|
| Generator        | Fuel-curve + runtime calculation      | kVA, hours, fuel price                 | High (HOMER-style + Nigerian studies)  | Needs reasonable fuel-curve data        | Monthly fuel cost, cost/kWh, efficient load range |
| Pump / Motor     | Affinity laws + efficiency curves     | Size, hours, energy cost               | Medium–High                            | Accuracy depends on input quality       | Monthly cost, efficiency flag               |
| Small Wind       | Weibull + power-curve calculation     | Avg wind speed / location, turbine size| High (Nigerian wind studies + WIS)     | Site-specific; early screening only     | Annual kWh estimate, rough payback          |
| Multi-Energy     | Hybrid cost ranking                   | Sources used + basic costs             | Medium                                 | Early versions less precise             | Ranked cost options across sources          |

---

## 7. Research Gaps That Remain Open

- Fully sparse, user-input-only versions of generator and pump tools tailored for everyday Nigerian households are less developed than the underlying mathematics.
- User-friendly, naira-based small-wind payback calculators at household/small-business scale are limited.
- Integrated pure-software tools that treat electricity + generator + mechanical loads in one simple interface are still rare.
- Local validation of these sparse methods on large numbers of real Nigerian users is still needed.

---

## 8. Practical Research Recommendation

The strongest pure-software opportunities across other forms are:

1. Generator fuel-cost and efficiency-range calculator (highest immediate practicality).
2. Pump/motor monthly-cost and efficiency estimator.
3. Simple small-wind screening tool using public or user-estimated wind data.
4. Multi-source ranking layer that sits on top of the electrical bottom-up work already researched.

All of these can be built without sensors, use the same sparse-data philosophy, and support the calculation + awareness goals from the original voice note.

---

**Sources are real published studies, standard engineering models, and Nigerian-specific research. All methods described can operate as pure software.**

This document expands the research across the other energy forms and can be used together with the earlier electrical bottom-up document.
