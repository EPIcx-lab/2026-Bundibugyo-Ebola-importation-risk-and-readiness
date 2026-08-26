# Relative Ebola importation-risk calculation

The Jupyter notebook `risk_calculation.ipynb` calculates country-level relative importation risk for the study *Importation risk and preparedness priorities across Africa in the 2026 Bundibugyo Ebola outbreak: a modelling study*. It implements the framework described in Appendix 2, section “Definition of importation risks.”


## Input files

The notebook expects the following CSV files in its working directory. The paths can be changed in the configuration cell at the beginning of the notebook.

| File | Required columns | Description |
|---|---|---|
| `popUGA.csv` | `zone`, `pop` | Population of affected Ugandan ADM3 units. |
| `popCOD.csv` | `zoneCode`, `pop` | Population of affected DRC health zones. |
| `cases.csv` | `HZ`, `ZSCode`, `cases` | Observed cases by affected area. |
| `mAirCOD.csv` | `zoneCode`, `dest`, `pax` | Air flows from DRC health zones to destination countries. |
| `mAirUGA.csv` | `zoneCode`, `dest`, `pax` | Air flows from Ugandan ADM3 units to destination countries. |
| `mLandCOD.csv` | `zoneCode`, `dest`, `pax` | Land flows from DRC health zones to destination countries. |
| `mLandUGA.csv` | `zoneCode`, `dest`, `pax` | Land flows from Ugandan ADM3 units to destination countries. |

Origin identifiers must be consistent across the case, population, and mobility files. Population values must be positive, and case and passenger-flow values must be numeric and non-negative.

## Epidemiological scenarios

The notebook evaluates two scenarios:
- **Observed cases:** uses the case counts reported in `cases.csv`.
- **Estimated cases:** divides observed cases by the reporting rate assigned to the relevant province or country.

Areas without a specified mapping are assigned a reporting rate of 1.0. There is no separate parameter file, reporting is specified in the code.


## Calculation

The `compute_risk()` function implements Appendix 2 Equations 7–19. For each affected origin $z$, destination $d$, and mobility mode $k$ (air, land, or combined), it calculates:

1. **Incidence in the origin area**

   $$e_z = \frac{C_z}{P_z}$$

   where $C_z$ is the case count and $P_z$ is the population.

2. **Total outbound mobility**

   $$n_z^k = \sum_d M_{zd}^k$$

   where $M_{zd}^k$ is mobility from origin $z$ to destination $d$.

3. **Incidence- and mobility-weighted origin contribution**

   $$\alpha_z^k = \frac{n_z^k e_z}{\sum_j n_j^k e_j}$$

4. **Destination share of each origin's mobility**

   $$r_{zd}^k = \frac{M_{zd}^k}{n_z^k}$$

5. **Relative importation risk for each destination**

   $$R_d^k = \sum_z r_{zd}^k\alpha_z^k$$

Air, land, and combined risks are normalized independently. The combined score is calculated from the sum of air and land flows; it is not the sum or average of the separately normalized air and land scores.

The notebook excludes destinations `COD` and `UGA`. As currently written, it also removes rows with zero land flow, meaning that air-only origin–destination routes are excluded from the air and combined calculations.

## Outputs

Running the notebook creates:

| File | Description |
|---|---|
| `riskTot_observedCases.csv` | Relative risks based on observed cases. |
| `riskTot_estimatedCases.csv` | Relative risks after adjustment for reporting rates. |

Each file contains:

- `dest`: destination-country identifier;
- `rTotAir`: relative air-mediated importation risk;
- `rTotLand`: relative land-mediated importation risk; and
- `rTot`: relative importation risk using combined mobility.

Values are proportions and should sum to approximately 1 within each risk column. Output rows are sorted alphabetically by destination.


## Software and execution

The notebook is written for **Python 3**, with the latest stable currently being Python 3.11.9.

Place the seven input files in the notebook's working directory, open `risk_calculation.ipynb`, and run all cells from top to bottom.

The calculation contains no stochastic operations therefore does not use or require a random seed.



