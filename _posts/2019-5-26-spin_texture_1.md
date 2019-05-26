---
layout: post
title: Calculate spin texture and band structure
---

## Pymatgen
### Obtain high-symmetry paths in reciprocal space for bandstructure calculations
```
from pymatgen.symmetry.bandstructure import HighSymmKpath
from pymatgen.io.vasp.inputs import Poscar
struct = Poscar.from_file("POSCAR").structure
kpath = HighSymmKpath(struct, symprec=0.0001)
kpath.get_kpoints()
lines = kpath.get_kpoints()
```

## Pyprocar
### Spin texture: Read PROCAR from a Spin-orbit calculation
```
import pyprocar
pyprocar.repair('PROCAR','PROCAR_repaired')
pyprocar.fermi2D('PROCAR_repaired', outcar='OUTCAR', energy= -1.5, st=True, rotation=[90, 0, 1, 0], savefig="spin_texture.pdf")
```
### Band structure: Read PROCAR from a Spin-orbit calculation
```
import pyprocar
pyprocar.repair('PROCAR','PROCAR_repaired')
pyprocar.bandsplot('PROCAR_repaired',outcar='OUTCAR', elimit=[-5,5], cmap='seismic', savefig="bands_plot.pdf")
```

