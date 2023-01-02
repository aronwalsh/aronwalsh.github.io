---
layout: post
title: Apple for science
date: 2012-04-22 10:11:50.000000000 +01:00
type: post
---
<p>For my research, Mac offers a nice compromise between the functionality of Windows and the power of Linux.  Chemistry comfort software like Chemdraw, Endnote, Mendeley, MS Office and LaTEX are all available. For scientific computing, there are a few quirks that you need to get around:</p>
<p>(i) Where is .bashrc? It hides in /etc/bashrc. For a splash of colour, add export TERM=xterm-color to your .bash_profile.</p>
<p>(ii) Package managers: no Yum, but <a href="http://www.macports.org/">Macports</a> and <a href="http://www.finkproject.org/">Fink</a> work well.</p>
<p>(iii) How to hide folders from Finder? Away from the command line, some folders really should remain hidden from view. One good example is the 'Microsoft_User_Data’ folder. Solution: SetFile -a V [folder]</p>
<p>(iv) Free useful applications? <a href="http://www.mendeley.com/">Mendeley</a> (references); <a title="http://www.geocities.jp/kmo_mma/crystal/en/vesta.html" href="http://www.geocities.jp/kmo_mma/crystal/en/vesta.html">Vesta</a> (visualisation); <a title="http://www.inkscape.org/" href="http://www.inkscape.org/">Inkscape</a> (vector art); <a title="http://www.gimp.org/" href="http://www.gimp.org/">Gimp</a> (bitmap art); <a title="http://openbabel.org/wiki/Main_Page" href="http://openbabel.org/wiki/Main_Page">Open Babel</a> (structure conversion);  <a title="http://www.geogebra.org/cms/en" href="http://www.geogebra.org/cms/en">GeoGebra</a> (geometric fun); <a title="http://www.xm1math.net/texmaker/" href="http://www.xm1math.net/texmaker/">Texmaker</a> (LaTex GUI).</p>
<p>(v) Free C and Fortran compilers? You would expect to have standard compilers installed by default. Well no, but fortunately the good people at <a title="http://hpc.sourceforge.net/" href="http://hpc.sourceforge.net/">Mac HPC</a> and <a title="http://www.macresearch.org/gfortran-leopard" href="http://www.macresearch.org/gfortran-leopard">Mac Research</a> are there to help. First you need to install XCode (from the AppStore, or directly from <a title="http://developer.apple.com/xcode/" href="http://developer.apple.com/xcode/">here</a>), and for the newer versions you need to install the <a href="https://developer.apple.com/downloads/index.action?=command%20line%20tools">command line tools</a> add-on separately. This sets up a basic gcc compiler. Binaries for the latest versions of gcc/gfortran are available <a title="http://hpc.sourceforge.net/" href="http://hpc.sourceforge.net/">here</a>. Be sure to download both binaries, and remember to add the path to your shell: export PATH=/usr/local/bin:$PATH</p>
<p>(vi) Materials modelling codes (FHI-AIMS, VASP, GULP)? For <a title="https://projects.ivec.org/gulp/" href="https://projects.ivec.org/gulp/">GULP</a>, an OSX binary is available. Once gfortran is installed, you can download and compile standard Unix distributions of <a title="http://www.tacc.utexas.edu/software_modules.php" href="http://www.tacc.utexas.edu/software_modules.php">GotoBLAS</a> and <a title="http://www.netlib.org/lapack/" href="http://www.netlib.org/lapack/">LAPACK</a>. For <a title="http://www.fhi-berlin.mpg.de/aims/" href="http://www.fhi-berlin.mpg.de/aims/">FHI-AIMS</a>, the makefile is straightforward, and the binary runs perfectly:</p>
<p>FC = gfortran -m64<br />
FFLAGS = -O2 -ffree-line-length-none<br />
F90FLAGS = $(FFLAGS)<br />
ARCH = arch_generic.f90<br />
LAPACKBLAS = /progs/lapack-3.2.1/lapack_LINUX.a /progs/GotoBLAS2/libgoto2-r1.13.a</p>
<p>For <a title="http://cms.mpi.univie.ac.at/vasp/" href="http://cms.mpi.univie.ac.at/vasp/">VASP 5</a>, the following gives a serial binary that works quite well:</p>
<p>FC = gfortran -m64<br />
OFLAG  = -O2<br />
FFLAGS = -ffree-form -ffree-line-length-none<br />
BLAS = /progs/GotoBLAS2/libgoto2-r1.13.a<br />
LAPACK = /progs/lapack-3.2.1/lapack_LINUX.a</p>
<p>In my experience OpenMPI works well, with efficient use of four cores on i7 iMacs, and as expected for the premium price, the Intel fortran compiler (and MKL libraries) result in a significant (~20%) performance boost.  Unfortunately, no non-commercial version of ifort is available for Macs.</p>
