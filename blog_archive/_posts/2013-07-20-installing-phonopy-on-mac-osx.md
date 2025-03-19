---
layout: post
title: Installing Phonopy on Mac OSX
date: 2013-07-20 08:29:56.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- My work
- Scientific computing
tags:
- DFT
- Free Energy
- Phonons
- Phonopy
- Python
meta:
  _edit_last: '35096388'
  geo_public: '0'
  _publicize_pending: '1'
author:
  login: aronwalsh
  email: aronjwalsh@gmail.com
  display_name: Aron
  first_name: Aron
  last_name: Walsh
permalink: "/2013/07/20/installing-phonopy-on-mac-osx/"
---
<p><a href="http://phonopy.sourceforge.net/#">"Phonopy is an open source package of phonon calculations based on the supercell approach."</a> It is a wonderful lattice dynamics package, developed by <a href="http://atztogo.github.io/">Prof. Atsushi Togo</a>, which links to a wide variety of electronic structure packages. Since it is only used for pre- and post-processing of phonons, it is light enough to run on your laptop.</p>
<p>There is an official Mac installation <a href="http://phonopy.sourceforge.net/MacOSX.html#install-macosx">guide</a> on the website, but I am not a big fan of MacPorts. Each step is simple, but there are many of them. I had help along the way from <a href="http://phaseboundary.com/">Adam Jackson</a>. The basic starting point is having XCode with command-line tools (note in OS 10.9, this requires you to run <code>xcode-select --install</code> after installation).</p>
<p><strong>1. Feed your Python with <a href="http://fonnesbeck.github.io/ScipySuperpack/">Scipy Superpack</a></strong><br />
- A script to install the latest versions of NumPy (linear algebra), SciPy (numerical algorithms), Matplotlib (plotting), iPython (interactive shell for python).<br />
<code>chmod +x install_superpack.sh</code><br />
<code>./install_superpack.sh</code></p>
<p><strong>2. Pip your Python </strong><br />
- Two more dependencies (and yet another package manager):<br />
<code>sudo easy_install pip</code><br />
<code>sudo pip install lxml</code><br />
<code>sudo pip install pyyaml</code></p>
<p><strong>3. C hack</strong><br />
- Depending on your version of XCode, the default C compiler might be Clang (which fails for Phonopy with "'omp.h' file not found"). Change it back to gcc by using <code>export CC=gcc</code>.</p>
<p><strong>4. Nearly there...</strong><br />
- You can now download <a href="http://sourceforge.net/projects/phonopy/">Phonopy</a>. Enter the main directory and run<br />
<code>python setup.py install --home=.</code><br />
and you will need to add its path to .bash_profile:<br />
<code>export PYTHONPATH=/Volumes/Unix/progs/phonopy/phonopy-1.7.1/lib/python</code></p>
<p><strong>5. Happy phonons</strong><br />
Once the phonopy bin is in your path, simply type <code>phonopy</code></p>
<p><code>aron$ phonopy</p>
<p>_ __ | |__   ___  _ __   ___   _ __  _   _<br />
| '_ \| '_ \ / _ \| '_ \ / _ \ | '_ \| | | |<br />
| |_) | | | | (_) | | | | (_) || |_) | |_| |<br />
| .__/|_| |_|\___/|_| |_|\___(_) .__/ \__, |<br />
|_|                            |_|    |___/</p>
<p>1.7.1</code></p>
