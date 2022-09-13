# subduction_GJI2022


This repository contains ASPECT Input files for the model described in Sandiford and Craig: _Plate bending earthquakes and the strength distribution of the lithosphere_, submitted to Geophysical Journal International, September 2022



## Notes on code version and modifactions

The model described in the paper was run using a version of ASPECT that was modified from the master branch with the following commit hash:

`64e71032ba0b6ba2e7e94e1acd9492fe6b8d8b56`

Modifications were made to the source code as described in the Supplementary Material.

These modifications are associated with the the conversion of experimental parameters to a stress-strain rate relationship based on effective quantities of the deviatoric tensors. Specifically, these modifications mean that the differential stress predicted by ASPECT will be the same as (notwithstanding numerical and resolution limitations) the stress estimated in typical formulation of Yield Strength Envelopes, for a given set of flow law parameters (i.e. pre-exponential factors) based on uniaxial experiments.

In the file `.../source/material_model/rheology/visco_plastic.cc`

the line

```c++
viscosity_pre_yield = (viscosity_diffusion * viscosity_dislocation)/
                                          (viscosity_diffusion + viscosity_dislocation);
```

is replaced by

```c++
viscosity_pre_yield = std::min(0.5*viscosity_diffusion, 0.5*viscosity_dislocation);

```

In the file `.../source/material_model/rheology/peierls_creep.cc`

the line

```c++
double viscosity_peierls = stress_term * arrhenius_term *strain_rate_term;

```

is replaced by:

```c++
double viscosity_peierls = 0.5*stress_term * arrhenius_term * strain_rate_term;
```
