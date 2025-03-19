---
layout: post
title: MacOS Mojave for Computational Materials Science in 2019
date: 2019-05-03 15:46:12.000000000 +01:00
---
<p><!-- wp:paragraph --></p>
<p>Where does two years go?  Yet another update of previous entries for setting up an Apple computer for scientific computing. It is not an absolute guide, but simply one way to get going. &nbsp;I started with a clean install of Mac OS 10.14.4 (Mojave) on a MacBook in May 2019.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
	
## Awaken UNIX

<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>The first step is to install the command line tools. Open a Terminal and type <code>make</code>&nbsp;which triggers the system to install command line tools module (basic UNIX commands including a&nbsp;<code>gcc</code>&nbsp;compiler). Be sure to set up your <a href="https://natelandau.com/my-mac-osx-bash_profile/">bash_profile</a>, <a href="http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/">ssh config</a>, and <a href="https://github.com/amix/vimrc">vimrc</a> files to make working faster and more comfortable.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:paragraph --></p>
<p>Later we will also need some compiler files that are no longer installed by default, so also run:</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:preformatted --></p>
<pre class="wp-block-preformatted">open /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg</pre>
<p><!-- /wp:preformatted --></p>
<p><!-- wp:list --></p>

## Basics

<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>My base applications include:<em> (standard office)</em> Dropbox,&nbsp;Slack,&nbsp;MS Office (<a href="https://www.imperial.ac.uk/admin-services/ict/training-resources/resources-and-services/microsoft-office-365/install-office-365/mac/">Imperial</a>&nbsp;link), <a href="https://tug.org/mactex/">Mactex</a>,&nbsp;Texmaker,&nbsp;<a href="https://www.mendeley.com/download-mendeley-desktop/">Zotero</a>; <em>(scientific computing)</em>&nbsp;latest&nbsp;<a href="http://hpc.sourceforge.net">gcc/gfortran</a>,&nbsp;<a href="https://www.iterm2.com/">iTerm</a>, <a href="https://macromates.com/">Textmate</a>, <a href="http://www.xquartz.org">XQuartz,</a>&nbsp;<a href="https://atom.io">Atom</a>, <a href="https://cyberduck.io">Cyberduck</a>, <a href="https://desktop.github.com">GitHub client</a>;&nbsp;<em>(materials modelling)</em>&nbsp;<a href="http://jp-minerals.org/vesta/en/">VESTA</a>, <a href="https://avogadro.cc">Avogadro</a>. For video and image editing <a href="https://www.ffmpeg.org">ffmpeg</a> and <a href="https://www.imagemagick.org">imagemagick</a> are essential.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>Python</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>Python package and environment maintenance can cause headaches, so I now use <a href="https://www.anaconda.com/download/">Conda</a>&nbsp;for Mac as standard.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>Fortran and C</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>It is possible to survive using gnu compilers and freely available maths libraries, but Intel Fortran and MKL tend to be faster and better tested (easier to compile). For non-commericial purposes Intel Composer is <a href="https://software.intel.com/en-us/qualify-for-free-software/student">free for OS X</a>. The C and Fortran packages install in a few clicks, but be sure source the variables in your <code>.bash_profile</code>:<br /><code>source /opt/intel/mkl/bin/mklvars.sh intel64<br />source /opt/intel/bin/compilervars.sh intel64</code></p>
<p>The outcome:<code><br /> % ifort --version<br /> ifort (IFORT) 19.0.3.199<br /></code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>Openmpi</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>To enable parallelism, I downloaded the latest source code of <a href="http://www.open-mpi.org/">openmpi</a> (4.0.1).<code><br /> ./configure -prefix=/usr/local/openmpi-4.0.1 CC=icc CXX=icpc FC=ifort F77=ifort FCFLAGS='-O1' CFLAGS='-O1' CXXFLAGS='-O1' --disable-dlopen<br /> make<br /> sudo make install</code><br />be patient… it can easily take 20 minutes. Finally add to your <code>.bash_profile</code>:<code><br /> export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/openmpi-4.0.1/lib/<br /> export PATH=./:/usr/local/openmpi-4.0.1/bin:$PATH<br /> export OMP_NUM_THREADS=1<br /> </code><br />The outcome:<code><br /> % mpif90 --version<br /> ifort (IFORT) 19.0.3<br /></code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>Phonopy</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>I use this open-source lattice-dynamics package a lot. The installation used to be multi-step, but Conda makes life easier:<code><br /></code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:preformatted --></p>
<pre class="wp-block-preformatted">conda install -c conda-forge phonopy h5py</pre>
<p><!-- /wp:preformatted --></p>
<p><!-- wp:paragraph --></p>
<p>To run:&nbsp;<code>phonopy</code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>VASP</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>I use a range of electronic structure packages, but&nbsp;<a href="https://www.vasp.at/">VASP</a> is the old reliable. I downloaded the latest version (5.4.4), which has streamlined the install process. Enter the main folder and <code><br />
cp ./arch/makefile.include.linux_intel ./makefile.include</code><br />The file needs to be modified to point to the correct compilers (I used icc, icpc, and mpifort). We will also remove <code>DscaLAPACK</code> from the pre-compiler options and set <code>SCALAPACK = &nbsp;</code>.&nbsp;There is one bug to fix before you type <code>make</code>: in&nbsp;<code>./src/lib/getshmem.c</code> add&nbsp;<code>#define SHM_NORESERVE 010000</code> to the end of the include statements.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:preformatted --></p>
<pre class="wp-block-preformatted">make</pre>
<p><!-- /wp:preformatted --></p>
<p><!-- wp:paragraph --></p>
<p>The outcome:<code><br /> % mpirun -np 4 ../vasp_std<br /> running on 4 total cores<br /> distrk: each k-point on 4 cores, 1 groups<br /> distr: one band on 1 cores, 4 groups<br /> using from now: INCAR<br /> vasp.5.4.4<br /></code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list --></p>
<ul>
<li><strong>ASE</strong></li>
</ul>
<p><!-- /wp:list --></p>
<p><!-- wp:paragraph --></p>
<p>The <a href="https://wiki.fysik.dtu.dk/ase/">atomistic simulation environment</a> is a useful set of Python tools and modules. It now installs, including the gui, in one command:</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:preformatted --></p>
<pre class="wp-block-preformatted">pip install --upgrade --user ase</pre>
<p><!-- /wp:preformatted --></p>
<p><!-- wp:paragraph --></p>
<p>The outcome:<code>ase-gui<br /></code></p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:image --></p>
<figure class="wp-block-image"><img src="{{ site.baseurl }}/assets/2019/05/ase.jpg" alt="ASE" /></figure>
<p><!-- /wp:image --></p>
<p><!-- wp:paragraph --></p>
<p>I will update with more codes and tools as I find time, and always happy to receive suggestions.</p>
<p><!-- /wp:paragraph --></p>
