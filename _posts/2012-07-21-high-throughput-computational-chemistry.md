---
layout: post
title: High-throughput computational chemistry
date: 2012-07-21 17:13:36.000000000 +01:00
---
<p>The landscape is rapidly changing in the world of electronic structure calculations. When I started in the area, even the optimization of a low symmetry crystal structure with a first-principles approach was a significant computational challenge. Now with computing cores become cheaper and more abundant, density functional theory calculations, especially with local or semi-local functionals, are becoming routine and can be tackled even with a (powerful) desktop processor such as the Intel i7. So what should we do with our high-performance computers?</p>
<p>This week there were two good examples of combinatorial computations, each different in their approach.</p>
<p><a href="http://pubs.rsc.org/en/content/articlelanding/2012/ee/c2ee22341d">"New Cubic Perovskites for Single- and Two-Photon Water Splitting using the Computational Materials Repository" led by Karsten Jacobson </a><br />
<em>Theory:</em> RPBE (structures); GLLB-SC (band gaps).<br />
<em>Concept:</em> Take a single structure type and substitute in 19000 elemental combinations; screen for properties related to photoelectrochemistry.<br />
<em>Result:</em> 20 candidate materials.<br />
<em>Comment:</em> An effective brute force approach, with the main limitation being the assumption of a single crystal structure (perovskite). This work is part of the <a href="https://wiki.fysik.dtu.dk/cmr/">Computational Materials Repository</a> project.</p>
<p><a href="http://prb.aps.org/abstract/PRB/v86/i1/e014109">"Prediction of A<sub>2</sub>BX<sub>4</sub> metal-chalcogenide compounds via first-principles thermodynamics" led by Alex Zunger</a><br />
<em>Theory:</em> PBE / PBE+U.<br />
<em>Concept:</em> Take a single material stoichiometry (A2BX4) and investigate 429 unreported materials in 40 structure types; screen for thermodynamic stability / accessibility.<br />
<em>Result:</em> 100 new and theoretically stable materials.<br />
<em>Comment:</em> Technically this is the more creative approach as the crystal structure is not constrained. In addition to 40 known structure types, a global structure optimization method is used to assess some compounds, and a range of magnetic configurations are also included for transition metals.  As a result, the computational cost quickly elevates from 429 material systems to &gt; 70000 calculations. This work is part of the <a href="http://www.centerforinversedesign.org/">Center for Inverse Design</a>.</p>
<p>These studies form part of a wider trend, with a number of codes and databases appearing over the past few years:</p>
<p><a href="http://nlshm-lab.jlu.edu.cn/~calypso.html">CALYPSO </a>(Global structure optimization)<br />
<a href="http://mysbfiles.stonybrook.edu/~aoganov/USPEX.html">USPEX </a>(Global structure optimization)<br />
<a href="http://xtalopt.openmolecules.net/">XTALOPT</a> (Global structure optimization)<br />
<a href="http://caldb.nims.go.jp/index_en.html">CompES</a> (Database)<br />
<a href="http://www.materialsproject.org/">Materials Project </a>(Database)</p>
<p>It was quickly realised after the initial hype for experimental combinatorial chemistry that it is not a cure-all approach, but as we have access to supercomputers with 100000s of processing cores,  combinatorial computational chemistry is becoming an increasingly powerful tool.</p>
