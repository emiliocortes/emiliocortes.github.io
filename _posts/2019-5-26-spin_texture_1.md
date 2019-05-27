---
layout: post
title: Calculate spin texture and band structure
---

## Band structure calculation
To obtain band structure first we need a converged charge density. Starting from that CHGCAR a non-selfconsistent calculation is performed keeping the charge density constant (ICHARG = 11). Line mode must be specified in the KPOINTS file along with the k-points path. This can be done using the pymatgen package (script from https://ma.issp.u-tokyo.ac.jp/en/app-post/1146)

```
from pymatgen.symmetry.bandstructure import HighSymmKpath
from pymatgen.io.vasp.inputs import Poscar, Kpoints

struct = Poscar.from_file("POSCAR").structure
kpath = HighSymmKpath(struct, symprec=0.0001)
kpts = Kpoints.automatic_linemode(divisions=10,ibz=kpath)
kpts.write_file("KPOINTS_path")
```

Then, the band structure along that path can be plotted 
```
from pymatgen.io.vasp.outputs import Vasprun
from pymatgen.electronic_structure.plotter import BSPlotter

vaspout = Vasprun("vasprun.xml")
bandstr = vaspout.get_band_structure(line_mode=True)
plt = BSPlotter(bandstr).get_plot(ylim=[-12,10])
plt.savefig("band.pdf")
```


## Spin texture: 
Spin texture can be obtained from a non-collinear run. The INCAR of a non-collinear run must contain the following tags
```
	LSORBIT = .TRUE.	! sets LNONCOLLINEAR=.TRUE. by defalut
	NBANDS = 2 * number of bands of collinear run
	ISYM = -1	! recommended in VASP manual
	SAXIS =   sx sy sz    ! global spin quantisation axis
```
Plot using pyprocar package
```
import pyprocar
pyprocar.repair('PROCAR','PROCAR_repaired')
pyprocar.fermi2D('PROCAR_repaired', outcar='OUTCAR', energy= -1.5, st=True, rotation=[90, 0, 1, 0], savefig="spin_texture.pdf")
```
