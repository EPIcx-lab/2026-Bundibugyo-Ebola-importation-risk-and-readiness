# Demo: Relative Ebola importation-risk calculation

This folder provides a fully reproducible demonstration of the relative importation-risk calculation used in *Importation risk and preparedness priorities across Africa in the 2026 Bundibugyo Ebola outbreak: a modelling study*.

**All input data included in this folder are synthetic and do not contain or reproduce the data used in the study.** They are provided solely to demonstrate that the analysis workflow can be executed and to illustrate its expected behaviour.

## Running the demo

All files required to run the demo are included in this folder.

For a detailed description of the input files, calculations, and equations implemented by the notebook, see the README in the `/code` folder.

## Expected output

The notebook produces the relative importation-risk outputs and associated risk map.

The **synthetic** inputs are constructed so that the expected relative importation risk is homogeneous across destinations, with the mobility contribution split equally between air and land (50% air and 50% land).

This expected result provides a simple check that the calculation pipeline is functioning correctly.

## Expected run time

The complete demo runs in a few seconds on a standard desktop or laptop computer.