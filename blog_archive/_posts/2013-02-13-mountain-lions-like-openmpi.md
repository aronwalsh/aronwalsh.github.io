---
layout: post
title: Mountain Lions like OpenMPI
date: 2013-02-13 14:05:09.000000000 +00:00
---
<p>Following the previous post on installing Fortran compilers in OSX 10.8 (<a title="Mountain Lions like Fortran" href="http://thelostelectron.wordpress.com/2012/07/28/mountain-lions-like-fortran/">Mountain Lions like Fortran</a>), the next step is to efficiently exploit all of those lovely i7 cores for computational chemistry.</p>
<p><strong>1. OpenMPI</strong><br />
- Forget any version that comes with XCode as you need to compile OpenMPI against your new Fortran installation.<br />
- Download the binary from http://www.open-mpi.org/ (current version 1.6.3).<br />
- Unzip and enter directory.<br />
- Run "./configure --prefix=/usr/local/openmpi-1.6.3 CC=gcc FC=ifort F77=ifort".<br />
- Run "make".<br />
- Run "sudo make install".<br />
- Add to your .bashrc or .bash_profile:<br />
export PATH=./:/usr/local/openmpi-1.6.3/bin:~/bin:/opt/intel/bin:$PATH<br />
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/openmpi-1.6.3/lib</p>
<p><em>Result:</em><br />
<em>$ which mpif90</em><br />
<em>/usr/local/openmpi-1.6.3/bin/mpif90</em></p>
<p><strong>2. FHI-AIMS </strong><br />
- A popular quantum chemistry package (from <a href="https://aimsclub.fhi-berlin.mpg.de/">here</a>; current version 081912).<br />
- Update the Makefile to include:</p>
<p>FC = ifort<br />
FFLAGS = -O3 -ip<br />
F90FLAGS = $(FFLAGS)<br />
ARCHITECTURE = Generic<br />
LAPACKBLAS = -L/opt/intel/mkl/lib \<br />
-I/opt/intel/mkl/include -lmkl_intel_lp64 \<br />
-lmkl_sequential -lmkl_core<br />
USE_MPI = yes<br />
MPIFC = mpif90</p>
<p>- Run "make mpi".<br />
- Enjoy parallel calculations,<em> e.g.</em> mpirun -np 4 fhi-aims</p>
<p><strong>3. VASP</strong><br />
- A popular materials modelling package (from <a href="http://cms.mpi.univie.ac.at/marsweb/">here</a>; current version 5.3.3).<br />
- In the main src folder, "cp makefile.linux_ifc_P4 Makefile".<br />
- Update the Makefile to include:</p>
<p>FC=mpif90<br />
FCL=$(FC)<br />
FFLAGS = -FR -assume byterecl<br />
OFLAG=-O3 -ip -ftz<br />
MKL=/opt/intel/mkl<br />
BLAS=-L/$(MKL)/lib -I/$(MKL)/include -lmkl_intel_lp64 \<br />
-lmkl_sequential -lmkl_core -lmkl_lapack95_lp64</p>
<p>- Run "make".<br />
- Enjoy parallel calculations,<em> e.g.</em> mpirun -np 4 vasp</p>
