CLAMP_14: Implementing a solution to the temperature plateau from clamp_12_##

Mesh: super coarse mesh (2688 cells)
OpenFOAM version: 9
Solver: clampPimpleFoam v5 and pimpleFoam

This system uses a loop in an Allrun file to:
1. in subcase 0; update the p and U fields with pimpleFoam,
2. then pass those fields to subcase 1; update the T fields with clampPimpleFoam v5 (pimple loop turned off -so p and U fixed).
