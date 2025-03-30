There's not much to read here, just tracking Mac setup for my own record.

# Materials Modelling on Apple 🍏

My first Apple laptop was a PowerBook running the PowerPC [G4](https://en.wikipedia.org/wiki/PowerPC_G4) processor. Intel x86 chips provided a performance jump, but they became efficient heat generators over time. The transition to Apple Silicon has been great for every day tasks, but operating system and library changes required a little effort to overcome bumps for scientific computing.

These steps were written while setting up a M4 Macbook Air with macOS Sequoia 15.3.2.

## Awaken UNIX

Call me lazy, but the App Store is convenient. It remembers software that I previously downloaded such as Microsoft Office, Slack, and Xcode, so it is a one-click installation for those. Not everything is there, but it saves time.

I use [homebrew](https://docs.brew.sh/Installation) for package management. Once Xcode is installed, it is just a one line installation and one more to install useful packages.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew install gcc openmpi scalapack fftw qd openblas hdf5 cmake
```

*zsh* is the default UNIX shell.  
Set up your [zprofile](https://craftofcoding.wordpress.com/2022/02/28/the-basics-of-configuring-the-z-shell-on-a-mac/), [ssh config](http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/), and [vimrc](https://github.com/amix/vimrc) files to make working faster and more comfortable. My `.zprofile` has some basic settings such as:

```bash
#stack size
ulimit -Ss unlimited

#general mac
alias ls='ls -G -ltr'
export TERM=xterm-color
alias cpu='echo time $(uptime) && echo $(sysctl -n hw.ncpu) cores on $(hostname)'

#c/fortran
export CC=gcc-14
export CXX=g++-14
export FC=gfortran
```

## Python

I previously used [conda](https://www.anaconda.com/download), but even as a basic user `conda` and `pip` are slow and too fond of conflicts. Time to try [uv](https://github.com/astral-sh/uv) with:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
uv python install 3.12
```

This installed python in 6.19s! Then for a basic setup:

```bash
uv venv .venv
source .venv/bin/activate
uv pip install numpy scipy matplotlib pandas seaborn scikit-learn torch jupyterlab ipykernel pymatgen smact
```

I am not sure if I can — or want to — code without an AI code assistant anymore. [GitHub Education](https://education.github.com/pack) gives access to [Copilot](https://github.com/features/copilot) in [VSCode](https://code.visualstudio.com), which is handy. 

To add your new Python environment as a kernel:

```bash
python -m ipykernel install --user --name=uvdef --display-name "Python (UVdef)"
```

## DFT: VASP

I use a few different density functional theory (DFT) packages, but [VASP](https://www.vasp.at) is a robust standard. Taking the latest version, I open the source code, and:

```bash
cd vasp.6.5.0
cp ./arch/makefile.include.gnu_omp makefile.include
```

`makefile.include` needs some tweaking, including:

- Change all instances of `gcc` to `gcc-14` and `g++` to `g++-14`.

Then modify the library links to point to the Homebrew installation with:

- `SCALAPACK_ROOT ?= /opt/homebrew/`
- `OPENBLAS_ROOT ?= /opt/homebrew/Cellar/openblas/0.3.29`
- `FFTW_ROOT  ?= /opt/homebrew/`

To enable HDF5 support, uncomment that block of text and edit:

- `HDF5_ROOT ?= /opt/homebrew/`

I have copied my working `makefile.include` over [here](https://gist.github.com/aronwalsh/78962582fd17d5b50365a62679d1bc8d).

Compile with:

```bash
make all
```

Check with:

```bash
mpirun -np 4 vasp_std
```

## DFT: FHI-AIMS

The all-electron DFT package [FHI-AIMS](https://fhi-aims.org) uses `cmake` for compilation setup, which has already been installed above via homebrew. Taking the latest version:

```bash
tar -xf fhi-aims.250320.tar.gz 
cd fhi-aims.250320
mkdir build
cd build
cp ../initial_cache.example.cmake ./initial_cache.cmake
```

I edited `initial_cache.cmake` with `gcc-14` and `g++-14`, along with:

```cmake
set(LIB_PATHS "/opt/homebrew/lib/ /opt/homebrew/Cellar/openblas/0.3.29/lib/" CACHE STRING "" FORCE)
set(LIBS "lapack blas scalapack" CACHE STRING "" FORCE)
```

The edited file is over [here](https://gist.github.com/aronwalsh/c8d601555d4b66473af516af8b4bb569).

Then simply:

```bash
cmake -C initial_cache.cmake ..
make -j 4
```

Check the outcome with:

```bash
mpirun -np 4 aims.250320.scalapack.mpi.x
```

Happy computing.
