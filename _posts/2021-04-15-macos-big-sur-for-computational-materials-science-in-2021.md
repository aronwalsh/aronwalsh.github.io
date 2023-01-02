---
layout: post
title: macOS Big Sur for Computational Materials Science in 2021
date: 2021-04-15 07:09:15.000000000 +01:00
---

Will this be my final update for Intel Macs? Given the initial performance reports of Apple silicon, I hope so. 
This is a refresh of previous posts covering a clean install of macOS 11.2.1 (Big Sur) on an iMac Pro in April 2021. 

## Awaken UNIX

The first step is to give `Terminal` some power. Go to `System Preferences; Security &amp; Privacy; Privacy; Developer Tools` and add `Terminal` to the apps that can run software locally.

A full `Xcode` install from the App Store is recommended. I used to open `Terminal` and type `make`, which triggers the system to install the command line tools module (basic UNIX commands including a `gcc` compiler), but you end up missing some essential components. 

Note that <em>zsh</em> is the default shell. 
Set up your <a href="https://natelandau.com/my-mac-osx-bash_profile/">zprofile</a>, <a href="http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/">ssh config</a>, and <a href="https://github.com/amix/vimrc">vimrc</a> files to make working faster and more comfortable. My `.zprofile` has some basic settings such as:

{% highlight ruby %}
alias ls='ls -G -ltr'
export TERM=xterm-color
alias cpu='echo time $(uptime) &amp;&amp; echo $(sysctl -n hw.ncpu) cores on $(hostname)''
{% endhighlight %}

Later you may need C libraries that are in a non-standard location, so I find it useful to create the following link:

{% highlight ruby %}
sudo mkdir /usr/local/include
sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/* /usr/local/include/
{% endhighlight %}

## Basics

My base applications include:<em> (standard office)</em> Box,&nbsp;Slack,&nbsp;MS Office (<a href="https://www.imperial.ac.uk/admin-services/ict/training-resources/resources-and-services/microsoft-office-365/install-office-365/mac/">Imperial</a>&nbsp;link), <a href="https://tug.org/mactex/">Mactex</a>,&nbsp;Texmaker,&nbsp;<a href="https://www.mendeley.com/download-mendeley-desktop/">Zotero</a>; <em>(scientific computing)</em>&nbsp;latest&nbsp;<a href="http://hpc.sourceforge.net">gcc/gfortran</a>,&nbsp;<a href="https://www.iterm2.com/">iTerm</a>, <a href="https://macromates.com/">Textmate</a>, <a href="http://www.xquartz.org">XQuartz,</a>&nbsp;<a href="https://atom.io">Atom</a>, <a href="https://cyberduck.io">Cyberduck</a>, <a href="https://desktop.github.com">GitHub client</a>;&nbsp;<em>(materials modelling)</em>&nbsp;<a href="http://jp-minerals.org/vesta/en/">VESTA</a>, <a href="https://avogadro.cc">Avogadro</a>. For video and image editing, <a href="https://www.ffmpeg.org">ffmpeg</a> and <a href="https://www.imagemagick.org">imagemagick</a> are essential.

## Python

Python package and environment maintenance can cause headaches, so I now use <a href="https://www.anaconda.com/download/">conda</a>&nbsp;(Python 3) as standard. This comes with all of the basic packages including <a href="http://jupyter notebook install">jupyter-lab</a>. Two commands are required to initialise conda properly after installation:

{% highlight ruby %}
source /opt/anaconda3/bin/activate
conda init zsh
{% endhighlight %}

## Fortran and C

It is possible to survive using gnu compilers and math libraries, but Intel C, Fortran and MKL tend to be faster and better tested (easier to compile). They now come free as part of the <a href="https://software.intel.com/content/www/us/en/develop/tools/oneapi.html">oneAPI suite</a>, which avoids the clunky licensing protocols of times past. 

There are two parts to install: the base toolkit includes MKL and the HPC Toolkit includes the compilers. The Mac version now comes with scalapack, which is a welcome bonus. 

The install is easy and needs just one addition to your `.zprofile`:

{% highlight ruby %}
source /opt/intel/oneapi/setvars.sh
{% endhighlight %}

Check the outcome with `ifort --version`.

## Openmpi

To enable parallelism, I downloaded the latest source code of <a href="http://www.open-mpi.org/">openmpi</a> (4.1.1).

{% highlight ruby %}
./configure -prefix=/usr/local/openmpi-4.1.1 CC=icc CXX=icpc FC=ifort F77=ifort FCFLAGS='-O1' CFLAGS='-O1' CXXFLAGS='-O1'
 make
 sudo make install
{% endhighlight %}

be patient… it can easily take 20 minutes.  

With my version of oneAPI, the openmpi compilation was successful but later library errors emerged when compiling other code using mpif90. The fix (following <a href="https://ntq1982.github.io/files/20200621.html">this post</a>) is to add &nbsp;to add `-Wl,` to the `wl` variable (`wl="-Wl,"`) in the libtool file&nbsp;after `configure' and before `make'.
Finally add to your `.zprofile`:

{% highlight ruby %}
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/openmpi-4.1.1/lib/
export PATH=./:/usr/local/openmpi-4.1.1/bin:$PATH
export OMP_NUM_THREADS=1
{% endhighlight %}

Check the outcome with `mpif90 --version`.

## Phonopy

I use this open-source lattice-dynamics package a lot. The installation used to be multi-step, but if you just want to run the code (and not actively develop), then `conda` makes life simple:

{% highlight ruby %}
sudo conda install -c conda-forge phonopy h5py
{% endhighlight %}

To run: `phonopy`

## DFT: VASP

I use a range of density functional theory (DFT) packages, but&nbsp;<a href="https://www.vasp.at/">VASP</a> is a robust standard. Taking the shiny 6.2.0, enter the main folder and `cp ./arch/makefile.include.linux_intel ./makefile.include`.
The file needs to be modified to point to the correct compilers (I used `icc`, `icpc`, and `mpifort`). MKL on Mac no longer has a "intel64" subfolder, you also need to adjust that link. The blacs library also changed name to `-lmkl_blacs_mpich_lp64`.

Although, mkl now includes scalapack, I found it produced a segmentation fault, so removed it by deleting `DscaLAPACK` from the pre-compiler options and setting `SCALAPACK = `.
There is still one issue to fix: in `./src/lib/getshmem.c`. Add `#define SHM_NORESERVE 010000`` to the end of the include statements. Then run `make`.

Check the outcome with `mpirun -np 4 vasp_std`.

## DFT: FHI-AIMS

The all-electron DFT package <a href="https://aimsclub.fhi-berlin.mpg.de/index.php">FHI-AIMS</a> now uses `cmake` for compilation setup, which can be downloaded from <a href="https://cmake.org">here</a>, and requires:<br />`sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install`. 

In the program directory run

{% highlight ruby %}
mkdir build
cd build
cp ../initial_cache.example.cmake ./initial_cache.cmake
cmake -C initial_cache.cmake ..'
{% endhighlight %}

I had to make the following edits to `initial_cache.example.cmake`: 

{% highlight ruby %}
set(LIB_PATHS "/opt/intel/oneapi/mkl/latest/libs" CACHE STRING "" FORCE)
set(LIBS "mkl_intel_lp64 mkl_sequential mkl_core mkl_blacs_mpich_lp64" CACHE STRING "" FORCE)
{% endhighlight %}

then simply compile using `make -j 4` (to use 4 cores).  Check the outcome with `mpirun -np 4 aims.210226.mpi.x` 

## ASE

The <a href="https://wiki.fysik.dtu.dk/ase/">atomistic simulation environment</a> is a useful set of Python tools and modules. It now installs, including the gui, with one command. Nothing more to say! 

{% highlight ruby %}
pip install --upgrade --user ase
{% endhighlight %}

The outcome can be checked with `ase-gui`.

<!-- wp:image -->
<figure class="wp-block-image"><img src="{{ site.baseurl }}/assets/2021/04/ase.jpg" alt="ASE" /></figure>
<!-- /wp:image -->

I will update with more codes and tools as I find time, and always happy to receive suggestions.

