---
layout: post
title: MacOS Sierra for Computational Materials Science in 2017
date: 2017-09-03 07:34:29.000000000 +01:00
---
<p>An update of previous entries for setting up an Apple computer for scientific computing. It is not an absolute guide, but simply one way to get going.  I started with a clean install of Mac OS 10.12 (Sierra).</p>
<ul>
<li><strong>Awaken UNIX</strong></li>
</ul>
<p>The first step is to install the command line tools. Open a Terminal and type <code>make</code> which triggers the system to install Xcode (if missing) and the command line tools module (basic UNIX commands including a <code>gcc</code> compiler). Be sure to set up your <a href="https://natelandau.com/my-mac-osx-bash_profile/">bash_profile</a>, <a href="http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/">ssh config</a>, and <a href="https://github.com/amix/vimrc">vimrc</a> files to make working faster and more comfortable.</p>
<ul>
<li><strong>Basics</strong></li>
</ul>
<p>My base applications include:<em> (standard office)</em> Dropbox, Slack, MS Office (<a href="https://www.imperial.ac.uk/admin-services/ict/training-resources/resources-and-services/microsoft-office-365/install-office-365/mac/">Imperial</a> link), <a href="https://tug.org/mactex/">Mactex</a>, Texmaker, <a href="https://www.mendeley.com/download-mendeley-desktop/">Mendeley</a>; <em>(scientific computing)</em> latest <a href="http://hpc.sourceforge.net">gcc/gfortran</a>, <a href="https://www.iterm2.com/">iTerm</a>, <a href="https://macromates.com/">Textmate</a>, <a href="http://www.xquartz.org">XQuartz,</a> <a href="https://atom.io">Atom</a>, <a href="https://cyberduck.io">Cyberduck</a>, <a href="https://desktop.github.com">GitHub client</a>; <em>(materials modelling)</em> <a href="http://jp-minerals.org/vesta/en/">VESTA</a>, <a href="https://avogadro.cc">Avogadro</a>. For video and image editing <a href="https://www.ffmpeg.org">ffmpeg</a> and <a href="https://www.imagemagick.org">imagemagick</a> are essential.</p>
<ul>
<li><strong>Python</strong></li>
</ul>
<p>Python package and environment maintenance can cause headaches, so this time I went with <a href="https://www.anaconda.com/download/">Conda</a> for Mac. I am happy with the results so far, and the standard install gives a good base set of packages.</p>
<ul>
<li><strong>Fortran and C</strong></li>
</ul>
<p>It is possible to survive using gnu compilers and freely available maths libraries, but Intel Fortran and MKL tend to be faster and better tested (easier to compile). For non-commericial purposes Intel Composer is now <a href="https://software.intel.com/en-us/qualify-for-free-software/student">free for OS X</a>. The package installs in a few clicks, but be sure source the variables in your <code>.bash_profile</code>:<br />
<code>source /opt/intel/mkl/bin/mklvars.sh intel64<br />
source /opt/intel/bin/compilervars.sh intel64</code><br />
<code></code><br />
The outcome:<code><br />
% which ifort<br />
/usr/local/bin/ifort<br />
% ifort --version<br />
ifort (IFORT) 17.0.4 20170411<br />
</code></p>
<ul>
<li><strong>Openmpi</strong></li>
</ul>
<p>To enable parallelism, I downloaded the latest source code of <a href="http://www.open-mpi.org/">openmpi</a> (2.1.1).<code><br />
./configure -prefix=/usr/local/openmpi-2.1.1 CC=icc FC=ifort F77=ifort<br />
make<br />
sudo make install</code><br />
be patient… it can easily take 20 minutes. Finally add to your <code>.bash_profile</code>:<code><br />
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/openmpi-2.1.1/lib/<br />
export PATH=./:/usr/local/openmpi-2.1.1/bin:$PATH<br />
export OMP_NUM_THREADS=1<br />
</code><br />
The outcome:<code><br />
% which mpif90<br />
/usr/local/openmpi-2.1.1/bin/mpif90<br />
% mpif90 --version<br />
ifort (IFORT) 17.0.4 20170411<br />
</code></p>
<ul>
<li><strong>Phonopy</strong></li>
</ul>
<p>I use this open-source lattice-dynamics package a lot. The installation used to be multi-step, but Conda makes life easier (adapted from <a href="https://atztogo.github.io/phonopy/install.html">Togo's official guide</a>):<code><br />
% conda install numpy scipy h5py pyyaml matplotlib openblas<br />
% git clone https://github.com/atztogo/phonopy.git<br />
% export CC=gcc<br />
% cd phonopy<br />
% python setup.py install --user </code></p>
<p>To run: <code>phonopy</code></p>
<p>If harmonic phonons are not enough for you, then <a href="http://atztogo.github.io/phono3py/index.html">Phono3py</a> lets you calculate phonon-phonon interactions, but it gets very expensive to compute.<code><br />
% git clone https://github.com/atztogo/phono3py.git<br />
% cd phono3py<br />
% python setup.py install --user</code></p>
<p>To run: <code>phono3py</code></p>
<ul>
<li><strong>VASP</strong></li>
</ul>
<p>I use a range of electronic structure packages, but <a href="https://www.vasp.at/">VASP</a> is the old reliable. I downloaded the latest version (5.4.4), which has streamlined the install process. Enter the main folder and <code><br />
cp ./arch/makefile.include.linux_intel ./makefile.include</code><br />
The file needs to be modified to point to the correct compilers (I used icc, icpc, and mpifort). We will also remove <code>DscaLAPACK</code> from the pre-compiler options and set <code>SCALAPACK =  </code>. There is one bug to fix before you type <code>make</code>: in <code>./src/lib/getshmem.c</code> add <code>#define SHM_NORESERVE 010000</code> to the end of the include statements.</p>
<p>The outcome:<code><br />
% mpirun -np 4 ../vasp_std<br />
running on 4 total cores<br />
distrk: each k-point on 4 cores, 1 groups<br />
distr: one band on 1 cores, 4 groups<br />
using from now: INCAR<br />
vasp.5.4.4<br />
</code></p>
<ul>
<li><strong>ASE</strong></li>
</ul>
<p>The <a href="https://wiki.fysik.dtu.dk/ase/">atomistic simulation environment</a> is a useful set of Python tools and modules. It now installs, including the gui, in one command:<code><br />
</code><code>pip install --user ase gpaw</code></p>
<p>The outcome:<br />
<code>ase-gui<br />
</code></p>
<p><img class="alignnone" src="{{ site.baseurl }}/assets/2017/09/ase.jpg" alt="ASE" width="250" height="280" /></p>
<p>I will update with more codes and tools as I find time, and always happy to receive suggestions.</p>
