CLAMP_17: A fully functional, if not a little temperamental, model

Mesh: super coarse mesh (2688 cells)
OpenFOAM version: 9
Solver: clampPimpleFoam v5 and pimpleFoam

This system uses a loop in an Allrun file to:
1. in subcase 01_wtw; update the p and U fields with pimpleFoam. 'wtw' = 'ltr' if the flow is left-to-right, or 'rtl' if the flow is right-to-left. The only difference between these sub-sub-cases is the geometry, which only have the inlet and outlet swapped.
2. The p and U fields are then passed to subcase 02_wtw. Here, the T fields are solved with clampPimpleFoam v5 (pimple loop turned off -so p and U fixed). Again, there are two sub-sub-cases with opposite inlet and outlets.

There were a few tricks for getting this to work.
- the p and U fields are solved for a 90 second period. Openfoam uses an interpolation of time variable inlet conditions. A linear interpolation is used here. This means that the inlet value changes at each time step, in a straight line between U at each point of observation (every 15 minutes). This resulted in the problem that when U was near 0 and of an opposite sign to the previous period, a 'ltr' velocity would be applied to the 'rtl' geometry - the flow at the inlet would be out of the domain. To have a single U at the inlet over these 90 seconds, the data file was extended to have a U value at the start and end of each 90 second simulation period.
- the solver doesn't like periods of 0 velocity. The work around here was to change Ux = 0 to 0.01. Issues (divergence) tended to appear when, after a peiod of zero velocity, velocity went to a low velocity (eg 0.04). To overcome this, the solver was run with a fixed time step. It slows the simulation down, but at least it keeps it going.
