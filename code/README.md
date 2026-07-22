The notebook risk_calculation.ipynb computes Ebola importation risk for every destination country, following Appendix #2 of the paper (section "Definition of importation risks").

Loads population and mobility flows (air/land/total) for every affected health zone, plus case counts per zone. Cases are also corrected for reporting rate (different per province) to get an "estimated" scenario on top of the raw "observed" one.

The function compute_risk() implements Eqs. 13-19 and 7-12 of the appendix:
- incidence per zone (e = cases/pop, Eq. 13)
- total outbound mobility from each zone (Eq. 14-16)
- alpha weight of each zone, i.e. mobility weighted by incidence (Eq. 17-19)
- the importation risk from each zone's mobility going to each destination (Eq. 7-9)
- weighted sum over all zones = final risk score per destination (Eq. 10-12)

All computed separately for air, land, and total.


