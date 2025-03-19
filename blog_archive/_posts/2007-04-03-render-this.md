---
layout: post
title: Render this...
date: 2007-04-03 16:45:34.000000000 +01:00
---
<p>At work I run Windows XP on my desktop and SSH into our linux systems (with -term facilitated by Exceed). 
	
Q. What are the best visualization options for viewing structure and charge density from VASP/Wien2K? I have previously used Materials Studio by Accelrys. It’s slow, expensive, eats memory and can’t export vector images. It’s almost worthy of being designed by Microsoft. Here are some freeware options I have been playing with recently:</p>
<p>(a) <a title="http://www.xcrysden.org/" href="http://www.xcrysden.org/">XCrySden</a></p>
<div>
<div>
<div><a href="http://thelostelectron.files.wordpress.com/2007/04/xcrysden.jpg"><img title="xcrysden" src="{{ site.baseurl }}/assets/2007/04/xcrysden.jpg" alt="" width="188" height="150" /></a></div>
</div>
</div>
<p>Originally aimed at Wien2K, but there are plenty of scripts around to convert VASP files. A very powerful program. It can automatically visualize the Brillouin zone for your cell, display good structures and produces some of the best isosurfaces I’ve seen. The EPS export didn’t work for me when I tried, but it warned me it would be tough with lighting turned on. The main problem is that it is a bit sluggish when run over SSH.</p>
<p>(b) <a title="http://homepage.mac.com/fujioizumi/visualization/VENUS.html" href="http://homepage.mac.com/fujioizumi/visualization/VENUS.html">VEND</a></p>
<div>
<div>
<div><a href="http://thelostelectron.files.wordpress.com/2007/04/vend.jpg"><img title="vend" src="{{ site.baseurl }}/assets/2007/04/vend.jpg" alt="" /></a></div>
</div>
</div>
<p>I had never heard of this until a few weeks ago when I started using the excellent <a title="http://www.geocities.jp/kmo_mma/crystal/en/vics.html" href="http://www.geocities.jp/kmo_mma/crystal/en/vics.html">VICS-II</a> for viewing my structure files. VICS-II arose from the VENUS suite which aims to satisfy all your modeling needs. It runs natively on windows and links in with an array of electronic structure programs. Fast and impressive. Exporting isosurfaces as vectors is quick and with very good results. Puts expensive packages to shame. (My winner!) Update: VEND has been replaced by the amazing <a title="http://www.geocities.jp/kmo_mma/crystal/en/vesta.html" href="http://www.geocities.jp/kmo_mma/crystal/en/vesta.html">VESTA</a>!</p>
<p>(c) <a title="http://www.cmmp.ucl.ac.uk/~lev/codes/lev00/" href="http://www.cmmp.ucl.ac.uk/%7Elev/codes/lev00/">Lev00</a></p>
<div>
<div>
<div><a href="http://thelostelectron.files.wordpress.com/2007/04/lev00.jpg"><img title="lev00" src="{{ site.baseurl }}/assets/2007/04/lev00.jpg" alt="" width="188" height="150" /></a></div>
</div>
</div>
<p>In the eighties the only computer I had was a Spectrum ZX that my cousin gave me (after they fell out of vogue). I’m sure it could have run Lev00. That’s not to say it’s not a useful suite. It can readily treat spin density files, linear combinations of density files and a has a number of nifty analysis tools. The visualization is purely contour based. You specify the plane and the contour ranges, and a second later you have a picture. Nice if you’re in a retro mood, but I like bright colours and fancy buttons.</p>
<p>(d) Loose ends: I remember using <a title="http://vaspview.sourceforge.net/" href="http://vaspview.sourceforge.net/">Vaspview</a> at some stage and it worked somewhat, however when I downloaded the latest compilation today, it just kept freezing. I also attempted to install <a title="http://cms.mpi.univie.ac.at/odubay/p4vasp_site/news.php" href="http://cms.mpi.univie.ac.at/odubay/p4vasp_site/news.php">P4VASP</a>. It needs a lot of extra packages to be linked. Too much effort for the moment. <a title="http://schmeling.ac.rwth-aachen.de/user/bernhard/wxdragon.html" href="http://schmeling.ac.rwth-aachen.de/user/bernhard/wxdragon.html">WXDragon</a> is handy for checking CIF files and exporting directly as program input, but its visualization isn’t up to scratch. Poor quality pictures and limited functionality. There’s a new version due out soon which might improve things though.</p>
