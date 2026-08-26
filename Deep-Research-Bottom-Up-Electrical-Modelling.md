# Deep Research Document: Bottom-Up Modelling for Residential Electrical Energy
## Pure-Software Calculation Logic, Equations, Paper Summaries, Comparisons, and What Can Be Used

**Prepared for research supporting pure-software energy tools in Nigeria**  
**Focus:** Electrical energy tracking and calculation from sparse / user-provided data (no sensors required)

---

## 1. What Was Found and Could Be Used

### Key Finding 1 – Bottom-up modelling works with the exact data a pure-software tool can collect
Bottom-up models build a household’s electricity demand from the ground up using:
- Appliance ownership list
- Approximate daily/weekly hours of use
- Number of occupants
- Occupancy / activity patterns (from simple surveys or time-use data)
- Optional weather data for cooling loads

This matches the original conversation goal of software that helps people track and understand daily energy use (like a budget) and perform calculations without hardware.

**What can be used:** A pure-software calculator that asks the user for their appliances and approximate hours, then produces:
- Estimated monthly kWh and cost in naira
- Ranking of which loads cost the most
- Simple “what-if” savings if hours are reduced

### Key Finding 2 – Validated Nigerian implementation exists
A full bottom-up weather-sensitive residential demand model was built for Abuja, Nigeria (2020). It used the first Nigerian Time-of-Use Survey + Markov Chain for occupancy + appliance models + reanalysis weather data. Validation against a small metering trial gave:
- Correlation coefficient 0.97
- RMSE 0.04
- Percentage error ~6%

**What can be used:** The methodology (not the proprietary code) can be simplified into a practical software tool for households and small businesses. The validation numbers show the approach is credible in the Nigerian context.

### Key Finding 3 – ANN estimated-billing model is ready for adaptation
A Nigerian Artificial Neural Network model for estimated billing uses five simple inputs:
1. Type of apartment
2. Number of occupants
3. Average daily power supply hours
4. Scored categories of electrical appliances
5. Scored behavioural energy usage pattern

It achieved a combined R-value of 0.99923 when trained on metered customers and then applied to unmetered ones.

**What can be used:** The input structure and training approach can be turned into a user-facing “electricity budget estimator” that is more transparent than current Disco estimated bills.

### Key Finding 4 – Persistence of savings is the open problem
Feedback and information interventions reliably produce 4–6% average savings, but effects often fade. Pure-software tools need ongoing engagement design if the “budgeting” awareness is to last.

**What can be used:** Design the software so users regularly update simple logs and receive evolving rankings and goals – this is the research-supported way to keep the effect alive.

---

## 2. Calculation Logic of Bottom-Up Modelling (Step-by-Step)

### Core Idea
Total household electricity demand = sum of demand from every individual appliance and end-use, driven by when people are home and active.

### Typical Steps Used in Research

1. **Generate occupancy / activity profiles**
   - Use time-use survey data or simple user answers.
   - Markov Chain (or semi-Markov) models transition probabilities between states (asleep, active at home, away, cooking, etc.).
   - Result: for each hour (or 15-minute step), probability that the household is in a given state.

2. **Assign appliances to activities**
   - Refrigerator → always on (base load)
   - Lighting → linked to occupancy + daylight
   - TV / fan / iron / pump → linked to specific activities or user-declared hours
   - Air conditioner → linked to occupancy + temperature (weather-sensitive)

3. **Calculate power for each appliance**
   - Simple form used in many models:
     ```
     Energy (kWh) = Power rating (kW) × Hours of use × Duty cycle / Diversity factor
     ```
   - For cooling loads a weather-dependent term is added (temperature difference × cooling capacity).

4. **Aggregate**
   - Sum across all appliances for the household.
   - Optionally scale to many households using ownership statistics.

5. **Validate / adjust**
   - Compare against any available metered data or known monthly totals.
   - Adjust factors (availability of supply, load factor) for intermittent grid conditions.

### Example Equations from Literature

From West African / Nigerian-style models:

Total hourly residential demand:
```
P_h^d = P_h,u^d + P_h,r^d
```
(where u = urban, r = rural)

Appliance contribution is typically:
```
E_appliance = N × P_rated × t_use × u_factor
```
where
- N = number of appliances of that type
- P_rated = rated power
- t_use = hours of use
- u_factor = usage / diversity / availability factor

For unmetered / estimated billing (Nigerian regulatory style):
```
Estimated Energy (kWh) = √3 × V_L × I_L × PF × A_v × L_F / 1000
```
(for measured load cases) or weighted cluster averages for non-MD customers.

---

## 3. Full Paper / Study Summaries (Most Relevant)

**Paper A – Bottom-up weather-sensitive model for Abuja, Nigeria (Energy for Sustainable Development, 2020)**
- First Time-of-Use Survey for Nigerian households.
- Markov Chain generates occupancy profiles.
- Appliance models + reanalysis weather data produce cooling loads.
- Validated against metering trial: correlation 0.97, ~6% error.
- Explicitly designed for developing-country conditions with suppressed / intermittent demand.

**Paper B – ANN-Based Estimated Electricity Billing System (IEEE PES/IAS PowerAfrica, 2018)**
- Five simple inputs (apartment type, occupants, supply hours, appliance scores, behaviour scores).
- Trained on metered customers → applied to unmetered.
- Combined R = 0.99923.
- Directly addresses the unfair estimated-billing problem in Nigeria.

**Paper C – Bottom-up archetype models of Nigerian residential dwellings (Buildings & Cities, 2024)**
- Creates representative building archetypes across climate zones.
- Uses DesignBuilder / EnergyPlus for lighting, cooling and equipment.
- Focuses on national stock-level estimates but the same logic can be simplified to single-household level.

**Other supporting studies**
- Multiple Markov / time-use bottom-up models from Europe, US and Africa confirm the same calculation structure works across climates.
- Hybrid bottom-up + top-down models exist for West African countries and can supply the statistical ownership rates needed for a software tool.

---

## 4. Comparison Table of Methods

| Method                        | Data Needed                          | Works with Sparse / Prepaid Data? | Validated in Nigeria / Africa? | Computational Complexity | Best Use in Pure-Software Tool                  |
|-------------------------------|--------------------------------------|-----------------------------------|-------------------------------|---------------------------|-------------------------------------------------|
| Bottom-up (appliance + hours) | Appliance list + hours + occupancy  | Yes                               | Yes (Abuja 2020)             | Low–Medium               | Household budget calculator & ranking          |
| ANN estimated billing         | 5 scored inputs                     | Yes                               | Yes (2018)                   | Medium                   | Fairer estimated monthly cost                  |
| Simple linear regression      | Bills + household characteristics   | Yes                               | Partial                      | Very Low                 | Quick baseline estimate                        |
| Markov + time-use             | Time-use survey or simplified logs  | Yes (with simplification)         | Yes                          | Medium                   | More realistic daily profiles                  |
| High-resolution NILM          | Continuous meter data               | No                                | Limited                      | High                     | Not suitable for pure-software start           |

---

## 5. Practical Recommendation for the Research / Startup

The strongest immediately usable finding is the combination of:

1. The Nigerian bottom-up methodology (Abuja paper) – simplified to user inputs.
2. The five-input ANN structure for estimated cost.
3. Ranking of loads by cost contribution.

A pure-software tool can ask the user for:
- List of main appliances and approximate daily hours
- Number of people in the house
- Typical hours of grid supply (or band)
- Current prepaid top-up or estimated monthly spend

Then output:
- Estimated breakdown of where the electricity money is going
- Ranking of the biggest loads
- Simple what-if savings if the user reduces hours on the top loads

This directly supports the original conversation goal of tracking energy like a budget and using calculation instead of only tips, while remaining pure software.

---

**Sources are real published papers and regulatory methodologies. All methods described can operate without continuous sensors.**

This document can be used as the research foundation for the electrical-energy part of the work.
