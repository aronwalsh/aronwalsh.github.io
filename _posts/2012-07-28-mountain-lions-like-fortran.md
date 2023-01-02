---
layout: post
title: Mountain lions like Fortran
date: 2012-07-28 13:38:13.000000000 +01:00
---
<p>A slow running laptop was a good excuse to play around with a fresh install of OSX 10.8 (Mountain Lion). The implementation of Xcode has been changing over the last few versions, so here are the few straight forward steps required to get command-line <a href="http://en.wikipedia.org/wiki/Fortran">Fortran</a> running on your Mac.</p>
<p><strong>1. Download Xcode from the App Store</strong><br />
- Run application to Install<br />
- Xcode Preferences -&gt; Downloads -&gt; Command Line Tools</p>
<p><strong>2. GNU Compilers</strong><br />
- Download 10.8 gcc and gfortran binaries and libraries from <a href="http://hpc.sourceforge.net">http://hpc.sourceforge.net</a>.<br />
- In terminal run "sudo tar -xvf gcc-mlion.tar -C /"<br />
- Ensure the line "export PATH=/usr/local/bin:$PATH" is in your .bash_profile. </p>
<p><em>Result:</em><br />
$ gfortran --version<br />
GNU Fortran (GCC) 4.8.0 20120722 (experimental)<br />
$ gcc --version<br />
gcc (GCC) 4.8.0 20120722 (experimental)</p>
<p><strong>3. Intel Compilers (Fortran Composer 2011)</strong><br />
- Install package.<br />
- For install environment choose "Command line install only".<br />
- If write permission issues arise, run "sudo chmod u+rwx /Users/Shared/Library/Application\ Support/".<br />
- source /opt/intel/bin/ifortvars.sh intel64<br />
- source /opt/intel/mkl/bin/intel64/mklvars_intel64.sh<br />
- Run once as "sudo ifort" to overcome some permissions issues with the libraries.</p>
<p><em>Result:</em><br />
$ ifort --version<br />
ifort (IFORT) 12.1.0 20111011</p>
<p><em>Example Makefile (<a href="https://aimsclub.fhi-berlin.mpg.de/">FHI-AIMS</a>):</em><br />
FC = ifort<br />
FFLAGS = -O3 -ip<br />
F90FLAGS = $(FFLAGS)<br />
ARCHITECTURE = Generic<br />
LAPACKBLAS = -L/opt/intel/mkl/lib \<br />
-I/opt/intel/mkl/include -lmkl_intel_lp64 \<br />
-lmkl_sequential -lmkl_core</p>
