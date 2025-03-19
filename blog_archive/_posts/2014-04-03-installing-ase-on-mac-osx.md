---
layout: post
title: Installing ASE on Mac OSX
date: 2014-04-03 21:07:14.000000000 +01:00
---
<p><a href="https://wiki.fysik.dtu.dk/ase/overview.html">"ASE is an Atomistic Simulation Environment written in the Python programming language with the aim of setting up, steering, and analyzing atomistic simulations."</a> It links to a wide variety of electronic structure packages and automates many tedious processes. Best of all it's free and open source.  </p>
<p>There is a Homebrew installation option, but sometimes it is nice to know what you are installing. The basic starting point is a system with XCode and SciPy (following the <a href="http://thelostelectron.wordpress.com/2013/07/20/installing-phonopy-on-mac-osx/">Phonopy installation guide</a>). </p>
<p><strong>1. Feed your Python with <a href="http://sourceforge.net/projects/macpkg/files/PyGTK/2.24.0/PyGTK.pkg/download">GTK</a></strong><br />
- An "object-oriented widget toolkit" in a simple self-installing package. </p>
<p><strong>2. Water your Python with <a href="http://ethan.tira-thompson.com/Mac_OS_X_Ports.html">Libpng</a></strong><br />
- A png reference library in a simple self-installing package. </p>
<p><strong>3. Download &amp; Link <a href="https://wiki.fysik.dtu.dk/ase/download.html#download">ASE</a> </strong><br />
<code> tar -zxf python-ase-3.8.0.3420.tar.gz<br />
ln -s python-ase-3.8.1.3440 ase<br />
cd ase<br />
python setup.py install --user </code></p>
<p><strong>4. Nearly there...</strong><br />
- Add to your .bash_profile (with the right pathway):<br />
<code> export PYTHONPATH=$HOME/progs/ase:$PYTHONPATH<br />
export PATH=$HOME/progs/ase/tools:$PATH </code></p>
<p><strong>5. Test (and have fun)! </strong><br />
<code> mkdir tmp; cd tmp<br />
python -c "from ase.test import test; test(verbosity=2, display=False)" 2&gt;&amp;1 | tee testase.log </code></p>
<p>OUTPUT: <code><br />
Ag-Cu100.py (ScriptTestCase) ... ok<br />
CO2_Au111.py (ScriptTestCase) ... ok<br />
COCu111_2.py (ScriptTestCase) ... ok<br />
...<br />
</code></p>
