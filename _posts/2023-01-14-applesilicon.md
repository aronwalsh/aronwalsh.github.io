---
layout: post
title:  Apple Silicon for Computational Materials Science
date:   2023-01-14 16:11:22 +0000
---

My first Apple laptop was a PowerBook running the PowerPC [G4](https://en.wikipedia.org/wiki/PowerPC_G4) processor. The transition to Intel x86 chips provided many opportunities, but they lost their edge over time to become efficient heat generators. So far, I am very impressed by the ARM [M2](https://en.wikipedia.org/wiki/Apple_silicon#Apple_M2) processor and its 20 billion transistors. It does take some effort to get everything nicely configured.

<p align="center">
<figure class="wp-block-image aligncenter"><img src="{{ site.baseurl }}/assets/2023/cpu.png" alt="CPU" /></figure>
</p>

## Awaken UNIX

Call me lazy, but I find the App Store convenient. It remembers that I have downloaded standard software such as Microsoft Office, Slack, and Xcode on some other machine, so it is a one-click installation for those. Not everything is there, but it does save time.

I also rely on [homebrew](https://docs.brew.sh/Installation) for package management these days. Once I have Xcode installed, it is just a one line installation and one more to install a range of packages that I will use.

{% highlight ruby %}
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew install gcc openmpi scalapack fftw qd openblas hdf5 cmake
{% endhighlight %}

<em>zsh</em> is the default UNIX shell. 
Set up your [zprofile](https://craftofcoding.wordpress.com/2022/02/28/the-basics-of-configuring-the-z-shell-on-a-mac/), [ssh config](http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/), and [vimrc](https://github.com/amix/vimrc) files to make working faster and more comfortable.  My `.zprofile` has some basic settings such as:

{% highlight ruby %}
#stack size
ulimit -Ss unlimited

#general mac
alias ls='ls -G -ltr'
export TERM=xterm-color
alias cpu='echo time $(uptime) && echo $(sysctl -n hw.ncpu) cores on $(hostname)'

#c/fortran
export CC=gcc-12
export CXX=g++-12
export FC=gfortran
{% endhighlight %}

Later I will need C libraries that are in a non-standard location, so I find it useful to create the following link:

{% highlight ruby %}
sudo mkdir /usr/local/include
sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/* /usr/local/include/
{% endhighlight %}

It may be very specific to this moment, but I had an issue with `gcc-12` and `g++-12` not finding standard headers due to a mismatch with the Xcode version. 
After spotting the source of the issue with `cpp-12 -v`, there is a simple fix:

{% highlight ruby %}
sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX12.3.sdk /Library/Developer/CommandLineTools/SDKs/MacOSX13.sdk
{% endhighlight %}

## Python

I use [conda](https://www.anaconda.com/download/) as standard. This comes with the core python-related packages including [jupyter-lab](https://jupyter.org). It's straightforward once you download the Apple Silicon (not x86) installation file. Then you are free tp install all of your favourite packages, e.g.

{% highlight ruby %}
pip install pymatgen ase phonopy smact matminer sumo shakenbreak
{% endhighlight %}

## DFT: VASP

I use a range of density functional theory (DFT) packages, but [VASP](https://www.vasp.at) is a robust standard. Thanks to [Alex](https://utf.github.io) for some of the tips used in getting this to work. Taking the shiny 6.3.2 version, I untared the source code, and:

{% highlight ruby %}
cd vasp.6.3.2
cp ./arch/makefile.include.gnu_omp makefile.include
{% endhighlight %}

`makefile.include` needs some tweaking, including:

- Change all instances of `gcc` to `gcc-12` and `g++` to `g++-12`
 
- Add `-Dqd_emulate` to `CPP_OPTIONS`

and then modify the library links to point to the homebrew installation:

- Set `SCALAPACK_ROOT ?= /opt/homebrew/`

- Set `OPENBLAS_ROOT ?= /opt/homebrew/Cellar/openblas/0.3.21` 

- Set `FFTW_ROOT  ?=  /opt/homebrew/`

To enable HDF5 support, uncomment that block of text and edit `HDF5_ROOT  ?= /opt/homebrew/`. I have copied my working `makefile.include` over [here](https://gist.github.com/aronwalsh/78962582fd17d5b50365a62679d1bc8d).

`make all` and wait a few minutes. Check the outcome by running `mpirun -np 4 vasp_std` in the bin. You are treated to a warm hello characteristic of the vaspmaster.

<!-- wp:image -->
<figure class="wp-block-image"><img src="{{ site.baseurl }}/assets/2023/vasp.png" alt="ASE" /></figure>
<!-- /wp:image -->

## DFT: FHI-AIMS

The all-electron DFT package [FHI-AIMS](https://fhi-aims.org) uses `cmake` for compilation setup, which has already been installed above via homebrew. Taking the latest version:

{% highlight ruby %}
tar -xf fhi-aims.221103.tar.gz 
cd fhi-aims.221103
mkdir build
cd build
cp ../initial_cache.example.cmake ./initial_cache.cmake
{% endhighlight %}

I edited `initial_cache.cmake` to change `mpif90` to `gfortan` and `cc` to `gcc-12`, along with 

{% highlight ruby %}
set(LIB_PATHS "/opt/homebrew/lib/ /opt/homebrew/Cellar/openblas/0.3.21/lib/" CACHE STRING "" FORCE)
set(LIBS "lapack blas scalapack" CACHE STRING "" FORCE)
{% endhighlight %}

The edited file is over [here](https://gist.github.com/aronwalsh/c8d601555d4b66473af516af8b4bb569). Then, simply:

{% highlight ruby %}
cmake -C initial_cache.cmake ..
make -j 4
{% endhighlight %}

Check the outcome with `mpirun -np 4 aims.221103.scalapack.mpi.x` and happy computing.
