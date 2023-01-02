---
layout: post
title: El Capitan for Computational Materials Science in 2016
date: 2016-01-03 11:16:53.000000000 +00:00
---
<p>Every few years, I give my laptop a fresh start and remove all the debris (applications, libraries, updates) that have built up. This time I started with a clean install of Mac OS 10.11 (El Capitan).</p>
<ul>
<li><strong>Basics</strong></li>
</ul>
<p>The first step is to install the essentials including Dropbox, Evernote, Todoist, Xcode (with <code> xcode-select --install</code>), Slack, Mendeley, MS Office, <a href="http://hpc.sourceforge.net">gcc/gfortran</a>, <a href="http://stronginference.com/ScipySuperpack/">Python superpack</a>, VESTA, Transmission, <a href="https://tug.org/mactex/">Mactex</a>, Texmaker, <a href="http://www.unrarx.com">Unrar</a>, VLC, Adobe Creative Suite, <a href="https://www.iterm2.com/">iTerm</a>, <a href="https://macromates.com/">Textmate</a>, <a href="http://www.xquartz.org">XQuartz</a>.</p>
<ul>
<li><strong>Fortran</strong></li>
</ul>
<p>While it is possible to survive using gfortan and freely available maths libraries, Intel Fortran and MKL tend to be faster and better tested (easier to compile) in my experience. For non-commericial purposes Intel Composer is now <a href="https://software.intel.com/en-us/qualify-for-free-software/student">free for OS X</a>. The package installs in a few clicks, but be sure source the variables in your <code>.bash_profile</code>:<br />
<code> source /opt/intel/mkl/bin/mklvars.sh intel64<br />
source /opt/intel/bin/ifortvars.sh intel64</code><br />
Finally you will need to make the MKL fast fourier transforms (FFTs) for use in most solid-state simulation packages:<code><br />
cd $MKLROOT/interfaces/fftw3xf/<br />
sudo make libintel64 CC=gcc<br />
</code><br />
The outcome:<code><br />
Arons-Air-V:~ aron$ which ifort<br />
/usr/local/bin/ifort<br />
Arons-Air-V:~ aron$ ifort --version<br />
ifort (IFORT) 16.0.1 20151020<br />
</code></p>
<ul>
<li><strong>Openmpi</strong></li>
</ul>
<p>To enable parallelism, I downloaded the latest source code of <a href="http://www.open-mpi.org/">openmpi</a> (1.10.1).<code><br />
./configure -prefix=/usr/local/openmpi-1.10.1 CC=gcc FC=ifort F77=ifort<br />
make<br />
sudo make install</code><br />
be patient… it can easily take 20 minutes. Finally add to your <code>.bash_profile</code>:<code><br />
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/openmpi-1.10.1/lib/<br />
export PATH=./:/usr/local/openmpi-1.10.1/bin:$PATH<br />
</code></p>
<p>The outcome:<code><br />
Arons-Air-V:~ aron$ which mpif90<br />
/usr/local/openmpi-1.10.1/bin/mpif90<br />
Arons-Air-V:~ aron$ mpif90 --version<br />
ifort (IFORT) 16.0.1 20151020<br />
</code></p>
<ul>
<li><strong>Phonopy</strong></li>
</ul>
<p>We use this open-source lattice-dynamics package a lot in our research. There are a few more libraries to install first:<code><br />
sudo easy_install pip<br />
pip2 install lxml<br />
pip2 install pyyaml<br />
export CC=/usr/local/bin/gcc</code><br />
then after expanding the<a href="http://atztogo.github.io/phonopy/"> source code</a>, simply type:<code><br />
python setup.py install<br />
</code></p>
<p>The outcome:<code><br />
Arons-Air-V:~ aron$ phonopy<br />
        _<br />
  _ __ | |__   ___  _ __   ___   _ __  _   _<br />
 | '_ \| '_ \ / _ \| '_ \ / _ \ | '_ \| | | |<br />
 | |_) | | | | (_) | | | | (_) || |_) | |_| |<br />
 | .__/|_| |_|\___/|_| |_|\___(_) .__/ \__, |<br />
 |_|                            |_|    |___/<br />
                                      1.10.9<br />
</code></p>
<ul>
<li><strong>Phono3py</strong></li>
</ul>
<p>If harmonic phonons are not enough for you, then <a href="http://atztogo.github.io/phono3py/index.html">Phono3py</a> lets you calculate phonon-phonon interactions, but it gets very computationally expensive. We need to install hdf5 (for more efficient data management):<code><br />
pip2 install h5py</code><br />
and lapacke for faster code. Download the latest version of <a href="http://www.netlib.org/lapack/">lapack</a> and: <code><br />
cp make.inc.example make.inc<br />
make lapackelib</code><br />
Then you are ready to compile. Download <a href="http://atztogo.github.io/phono3py/install.html">Phono3py</a> and modify <code>setup3.py</code> to link to your compiled lapacke library.<code><br />
if platform.system() == 'Darwin':<br />
include_dirs += ['/Users/aron/Documents/progs/lapack/lapack-3.6.1/lapacke/include']<br />
extra_link_args = ['/Users/aron/Documents/progs/lapack/lapack-3.6.1/liblapacke.a']<br />
</code> followed by:<br />
<code>python setup3.py install<br />
</code><br />
The outcome: <code><br />
Arons-Air-V:~ aron$ phono3py<br />
        _                      _____<br />
  _ __ | |__   ___  _ __   ___|___ / _ __  _   _<br />
 | '_ \| '_ \ / _ \| '_ \ / _ \ |_ \| '_ \| | | |<br />
 | |_) | | | | (_) | | | | (_) |__) | |_) | |_| |<br />
 | .__/|_| |_|\___/|_| |_|\___/____/| .__/ \__, |<br />
 |_|                                |_|    |___/<br />
                                          1.10.9<br />
</code></p>
<ul>
<li><strong>VASP</strong></li>
</ul>
<p>While we use a range of electronic structure packages, <a href="https://www.vasp.at/">VASP</a> is the old reliable. I downloaded the latest version (5.4.1), which has streamlined the install process<code>.<br />
cp ./arch/makefile.include.linux_intel ./makefile.include</code><br />
which needs to be modified to point to the correct compilers (here gcc, ifort and mpifort). We will also remove <code>-DscaLAPACK</code> from the precompiler options and set <code>SCALAPACK = </code>. There are now three patches/bug fixes to install:<code><br />
patch -p1 &lt; patch.5.4.1.08072015<br />
patch -p1 &lt; patch.5.4.1.27082015<br />
patch -p1 &lt; patch.5.4.1.06112015</code><br />
and one fix to sort out a gcc error. To the file <code>./src/lib/getshmem.c</code> add one line at the end of the include statements<code>:<br />
#define SHM_NORESERVE 010000<br />
</code></p>
<p>The outcome:<code><br />
Arons-Air-V:test aron$ mpirun -np 4 ../vasp_std<br />
running on 4 total cores<br />
distrk: each k-point on 4 cores, 1 groups<br />
distr: one band on 1 cores, 4 groups<br />
using from now: INCAR<br />
vasp.5.4.1 24Jun15 (build Jan 02 2016 21:20:37) complex<br />
</code></p>
<ul>
<li><strong>ASE</strong></li>
</ul>
<p>The <a href="https://wiki.fysik.dtu.dk/ase/">atomistic simulation environment</a> is a useful set of Python tools and modules. It now installs, including the gui, in two lines:<code><br />
brew install pygtk<br />
pip install python-ase</code></p>
<p>The outcome:<br />
<code>ase-gui<br />
</code></p>
<p><img class="alignnone" src="{{ site.baseurl }}/assets/2016/01/ase.jpg" alt="ASE" width="250" height="280" /></p>
<p>I will update with more codes and tools as I find time.</p>
