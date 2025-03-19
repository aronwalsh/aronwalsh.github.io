---
layout: post
title: Ab initio in two thousand and eight
date: 2008-01-14 16:16:22.000000000 +00:00
type: post
---
<div>
<div>
<p>It’s a new year, and a good time to review the current status of periodic DFT simulation codes. If I'm missing some important codes, please comment.</p>
<p>a. (plane wave codes)<br />
<a title="http://cms.mpi.univie.ac.at/vasp/" href="http://cms.mpi.univie.ac.at/vasp/">VASP</a> (Commercial). Tested PAW/p.p. database. MD. Elastic band. Stress.<br />
<a title="http://www.abinit.org/" href="http://www.abinit.org/">ABINIT</a> (Academic). PAW/p.p. MD. Stress. TD-DFT. GW. Optical.<br />
<a title="http://www.castep.org" href="http://www.castep.org/">CASTEP</a> (Commercial) Elastic band. MD. Optical. Stress.<br />
<a title="http://www.quantum-espresso.org/" href="http://www.quantum-espresso.org/">QUANTUM-ESPRESSO</a> (Academic) Elastic band, MD. Next version: PAW, Hybrid-DFT.<br />
<a title="http://www.cpmd.org/" href="http://www.cpmd.org/">CPMD</a>  (Academic) Flexible MD, TD-DFT.b. (all electron codes)</p>
<p>b. (all electron codes)<br />
<a title="http://www.wien2k.at/" href="http://www.wien2k.at/">WIEN2K</a> (Commercial). LAPW. Advanced optical analysis. Limited Hybrid-DFT.<br />
<a title="http://exciting.sourceforge.net/" href="http://exciting.sourceforge.net/">EXCITING</a> (Academic). LAPW. Hybrid and full HF calculations.<br />
<a title="http://www.flapw.de" href="http://www.flapw.de/">FLEUR</a>(Academic). LAPW.c. (gaussian basis set codes)</p>
<p>c. (gaussian basis set codes)<br />
<a title="http://www.crystal.unito.it/" href="http://www.crystal.unito.it/">CRYSTAL</a> (Commercial). Hybrid-DFT, HF. Slow. Relativistic cores can be tough to find.</p>
<p>d. (linear scaling codes)<br />
<a title="http://icmab.cat/leem/siesta/" href="http://www.uam.es/departamentos/ciencias/fismateriac/siesta/">SIESTA</a> (Academic). Efficient numerical basis set. MD. P.P./basis set database is limited.<br />
<a title="http://www.conquest.ucl.ac.uk/" href="http://www.conquest.ucl.ac.uk/">CONQUEST</a> (Academic). Plane waves / numerical basis. Tight binding / full DFT.<br />
<a title="http://www.tcm.phy.cam.ac.uk/onetep/" href="http://www.tcm.phy.cam.ac.uk/onetep/">ONETEP</a> (Academic). Density matrix. Wannier functions.<br />
<a title="http://www.accelrys.com/products/mstudio/modeling/quantumandcatalysis/dmol3.html" href="http://www.accelrys.com/products/mstudio/modeling/quantumandcatalysis/dmol3.html">DMOL</a>(Commercial). Numerical Basis. All-electron/cores. Interaction cutoff. Optical.e. (transport codes)</p>
<p>e. (transport codes)<br />
<a title="http://www.smeagol.tcd.ie/" href="http://www.smeagol.tcd.ie/">SMEAGOL</a> (Academic) Linked into SIESTA.<br />
<a title="http://www.wannier-transport.org/" href="http://www.wannier-transport.org/">WANNIER-TRANSPORT</a>(Academic) Linked into Quantum-Espresso.f. (codes you’d never really want to use)</p>
<p>f. (codes you’d never really want to use)<br />
<a title="http://www.gaussian.com" href="http://www.gaussian.com/">GAUSSIAN</a> (Commercial). The workhouse, and nightmare to compile, molecular code has limited support for periodic systems. Just don’t publish the results against any other code, or you’ll be on their blacklist for life (<a title="http://www.bannedbygaussian.org" href="http://www.bannedbygaussian.org/">http://www.bannedbygaussian.org</a>).</p>
</div>
</div>
