---
layout: post
title: Too many authors spoil the manuscript?
date: 2014-02-22 23:57:37.000000000 +00:00
---
<p>I published a few single author papers in the short window between being a postdoc (working for the boss) and a faculty member (working for the group). I do love collaboration, especially when it goes beyond contributing data to shaping the narrative and presentation in a manuscript. As the <a href="http://blog.coudert.name/post/2014/02/21/Evolution-of-chemistry-writing-over-5-decades">average number of authors increases</a> a linear procedure of passing successive drafts from <em>X</em> to <em>Y</em> is both inelegant and inefficient*.</p>
<p>I have detested MS Word since the day <a href="http://en.wikipedia.org/wiki/Office_Assistant#Criticism_and_parodies">Clippy</a> appeared**, so let's disregard any progress by MS Office in the cloud. We all know real scientists use LaTeX (my cocktail is <a href="http://tug.org/mactex/">MacTeX</a>, <a href="http://www.xm1math.net/texmaker/">Texmaker</a> and occasionally <a href="http://macromates.com">Textmate</a>). Many social LaTeX websites have appeared, e.g. <a href="https://www.sharelatex.com/">Share Latex,</a> but they currently seem clunky in dealing with packages, libraries and figures. One DIY solution I have been using in my group is Git. It may not be perfect, but whatever works!</p>
<p><a href="http://en.wikipedia.org/wiki/Git_%28software%29">Git</a> is a revision control system used in code development. We have been putting codes and scripts <a href="https://github.com/WMD-group">online</a> using GitHub, which offers free public Git repositories. The protocol allows you to keep track of any file type, so for LaTeX it works just as well. Of course, you don't really want to put your draft manuscripts in the public domain, but <a href="https://bitbucket.org/">BitBucket</a> offers unlimited free private repositories for education. The procedure is very simple***:</p>
<ul>
<li><strong>Initiate the repository locally<br />
</strong></li>
</ul>
<p><code> mkdir 2014-02_snso<br />
cd 2014-02_snso<br />
git init </code></p>
<ul>
<li><strong>Create the repository online</strong></li>
</ul>
<p>On the BitBucket website, complete the new repository form:</p>
<p><a href="http://thelostelectron.files.wordpress.com/2014/02/bit1.jpg"><img class="aligncenter size-large wp-image-775" src="{{ site.baseurl }}/assets/2014/02/bit1.jpg" alt="Bit1" width="830" height="535" /></a></p>
<ul>
<li><strong>Push to the cloud!<br />
</strong></li>
</ul>
<p>BitBucket gives you instructions to link the new online repository to your local files. In my case (from the same directory as above):<br />
<code> git remote add origin git@bitbucket.org:aronwalsh/2014-02-snso.git<br />
git push -u origin --all # pushes up the repo and its refs for the first time<br />
git push -u origin --tags # pushes up any tags</code></p>
<ul>
<li><strong>Write, share and maintain<br />
</strong></li>
</ul>
<p>Now for the easy bit: writing. Firstly, give all coauthors access to the repository. You can then simply add (or modify) files in the folder on your local machine and, when you are ready, "push" the changes to the online server. The latter can be done either using the command line or a GUI such as the one provided by <a href="http://mac.github.com/">GitHub</a> or the more powerful <a href="http://www.sourcetreeapp.com/">SourceTree</a>. Git will keep track of all changes made and you have the options to merge different versions or reject specific changes (e.g. from the author who insists on American spelling). You also have the option to ignore certain files types including all the auxiliary files generated you compile the TeX. So far, the system has been productive for me and avoids those "conflicting copy" errors that arise when using Dropbox for co-editing a manuscript. If you already use Git for software development, it is highly recommended.</p>
<p><a href="http://thelostelectron.files.wordpress.com/2014/02/bit2.jpg"><img class="aligncenter size-large wp-image-782" src="{{ site.baseurl }}/assets/2014/02/bit2.jpg" alt="Bit2" width="830" height="537" /></a><code><br />
</code></p>
<p>*One exception to this is a very fruitful collaboration I have with Shiyou Chen at Fudan University. An 8 hour time difference can be ideal for sequential drafting.<br />
**Okay, I occasionally have to use Word when collaborating due to its ubiquity.<br />
***Firstly, create a <a href="https://bitbucket.org/account/signup/">BitBucket</a> account using your university email address.</p>
