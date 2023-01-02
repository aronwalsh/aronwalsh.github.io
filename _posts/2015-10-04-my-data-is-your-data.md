---
layout: post
title: My data is your data
date: 2015-10-04 16:49:27.000000000 +01:00
type: post
---
<p>Academics in the UK are just coming to terms with an <a href="http://www.hefce.ac.uk/pubs/year/2014/201407/">open access policy</a> for publications (from paid ‘gold’ to free ‘green’ university repositories).</p>
<p>What has received significantly less attention is the new UK research data policy. In my experience, raising this issue in conversation is met with blank expressions… <em>what data policy?</em></p>
<p>Some key points from <a href="https://www.epsrc.ac.uk/about/standards/researchdata/">https://www.epsrc.ac.uk/about/standards/researchdata/</a>. From 1st May 2015:</p>
<blockquote>
<div>Published research papers should include a short statement describing how and on what terms any supporting research data may be accessed.</div>
</blockquote>
<blockquote><p>The metadata must be sufficient to allow others to understand what research data exists, why, when and how it was generated, and how to access it.</p></blockquote>
<p>My university, like most others, has put together <a href="http://www.bath.ac.uk/research/data">policy and guidance documents</a> but they are quite generic and don't seem to have really filtered down to the researcher level.</p>
<p>In my field of computational materials science, there are now several options:</p>
<ul>
<li><a href="https://github.com/">GitHub</a> - my group has been using this a lot for research (DOIs can be generated via the EU-funded project <a href="https://guides.github.com/activities/citable-code/">Zenodo; </a>2GB limit per repository). Instead of building separate repositories for each paper, we have been collecting related information, e.g. <a href="https://github.com/WMD-Bath/Phonons">Phonons</a> and <a href="https://github.com/WMD-Bath/Crystal_Structures">Crystal Structures</a>.</li>
<li><a href="https://data.mendeley.com/">Mendeley Data</a> - a nice clean interface for uploading data and generating DOIs, but I haven’t seen any clear policy for storage limits or guaranteed data lifetimes.</li>
<li><a href="http://figshare.com/">Figshare</a> - this repository plays nice with raw datasets and multimedia (e.g. a hybrid perovskite <a href="http://figshare.com/articles/Methyl_Ammonium_Lead_Iodide_MAPI_Pervoskite_2x2x2_Supercell_MD/1061490">MD video</a>). The serious drawback is a 1GB storage limit (per free account) with a 250 MB file size limit.</li>
<li><a href="http://nomad-repository.eu/cms/">NoMaD</a> - a new respository to “host, organize and share materials data”. I have great hopes for this one, but at the moment the website is a little jaded, and the interface is light years behind the <a href="https://materialsproject.org/">Materials Project</a> (which serves a different purpose of being a single source database).</li>
</ul>
<p>Ideally, a standard protocol would be adopted in the community to avoid the individual ‘data dumps’ that university repositories enable in favour of a systematic and searchable community database. <a href="http://www.aiida.net/">Aiida</a> allows one to do this at the research group or collaborator level, but I hope that <a href="http://nomad-repository.eu/cms/">NoMaD</a> can build a critical mass of researchers (and sustained funding) to make this a reality.</p>
