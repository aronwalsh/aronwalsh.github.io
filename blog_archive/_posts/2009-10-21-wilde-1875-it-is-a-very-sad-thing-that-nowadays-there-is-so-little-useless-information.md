---
layout: post
title: 'Wilde 1875: “It is a very sad thing that nowadays there is so little useless
  information.”'
date: 2009-10-21 14:28:32.000000000 +01:00
---
<div>
<div>
<p>After some head scratching trying to get VASP 5 installed on <a title="http://www.hector.ac.uk/" href="http://www.hector.ac.uk/">Hector</a> (Cray XT3), I finally got a working binary. To be extended as issues arise:</p>
<p>Problem 1: VASP5 has a preference for the IFC compiler (once you use <a title="http://cms.mpi.univie.ac.at/vasp-forum/forum_viewtopic.php?2.5776" href="http://cms.mpi.univie.ac.at/vasp-forum/forum_viewtopic.php?2.5776">-heap-arrays</a>) and the Cray machine doesn't support it.</p>
<p>Solution 1: Go with PGF90. On my machine "-fast -O4  -Mipa=fast,inline" runs quite well, although for some features (such as DFPT), you have to reduce the optimization level to O2 to avoid segmentation faults.</p>
<p>Problem 2: PGF90 (version 8.02) compiles the code, appears to run a full SCF, but dumps out with a segmentation fault before writing out the end result.</p>
<p>Solution 2: This problem has occurred before: <a title="http://cms.mpi.univie.ac.at/vasp-forum/forum_viewtopic.php?2.5588.10" href="http://cms.mpi.univie.ac.at/vasp-forum/forum_viewtopic.php?2.5588.10">http://cms.mpi.univie.ac.at/vasp-forum/forum_viewtopic.php?2.5588.10</a> ; the fix involves inserting half a dozen lines into the paw.F file to sort out some variable allocations that PGF90 doesn't appreciate. I have had no problems since. (Click for the <a title="http://web.mac.com/aronwalsh/random/paw-fixed.F" href="http://web.mac.com/aronwalsh/random/paw-fixed.F">updated paw.F file</a>).</p>
<p>Problem 3: The general approach to obtain a band structure is to run an SCF calculation (on a uniform k-point grid) and then fix the potential for the band structure k-points to give a nice (and cheap) E vs k curve. With the addition of non-local exchange (hybrid DFT functionals), this doesn't work in VASP. Actually, better phrased, the calculation will run, but you will have a set of chaotic eigenvalues, as k-points are no longer independent.</p>
<p>Solution 3: This fix is not ideal, but it is possible to include the additional band structure k-points in the original SCF calculation, i.e. in the KPOINTS file, but with weighting zero. You pay the computational penalty of calculating the extra k-points explicitly, but they don't contribute to the potential or total energy.  It is possible to do it the traditional way in CASTEP, so hopefully this will be integrated into VASP in the near future.</p>
<p>Problem 4: While testing processor scaling, I noticed a strange energy drift which sometimes appears with hybrid functionals for large supercells. For a static cell SCF calculation, starting from a random wavefunction, you can converge to different total energies (&gt; 0.1 eV per cell) at large numbers of processors (&gt; 128 cores).</p>
<p>Solution 4: From looking at the total energy breakdown, the electrostatic and atomic energies are exact, but the drift arises from a combination of differences in the exchange-correlation and Hartree energies. Restarting the job, to re-converge the wavefunction appears to help. Just something to be aware of and to check, especially if you are just doing a single point energy reference, e.g. for a bulk supercell before you introduce a defect.</p>
</div>
</div>
