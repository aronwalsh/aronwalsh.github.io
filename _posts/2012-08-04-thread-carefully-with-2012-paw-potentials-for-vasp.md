---
layout: post
title: Thread carefully with 2012 PAW potentials for VASP
date: 2012-08-04 00:07:00.000000000 +01:00
---
<p>The main advantage of the VASP code is a reliable set of pseudopotentials covering the entire periodic table. Earlier this year a new set was released with additional potentials optimised for GW calculations. The old potentials remained largely unchanged according to the release document: "In most cases the potentials are literally identical to the previous releases".</p>
<p>After struggling to understand some peculiar results, I realised that the last statement is not always true. The culprit appears to be related to <a href="http://cms.mpi.univie.ac.at/vasp/vasp/RPACOR_tag.html">core charges and corrections</a>, which has changed for quite a few elements even for the LDA/PBE sets.</p>
<p>Using VASP 5.2.12 with identical POSCAR, KPOINT and INCAR files, the following results were obtained for oxygen (triplet state, 500 eV planewave cutoff, PBEsol).</p>
<p><em>(a) Old POTCAR "PAW_PBE O 08Apr2002"</em><br />
Atom:   -0.974485 eV<br />
Molecule: -9.048636 eV<br />
Binding: -7.099666 eV</p>
<p><em>(b) New POTCAR "PAW_PBE O 08Apr2002"</em><br />
Atom: -1.571040 eV<br />
Molecule: -10.239136 eV<br />
Binding: -7.097056 eV</p>
<p>Clearly the energy shifts introduced can cancel for a balanced reaction, but if you mistakenly combine calculations using both sets (which appear the same in the header!), you will run into problems.</p>
