---
layout: post
title: One-pot recipe for small polaron localisation
date: 2020-08-04 09:40:48.000000000 +01:00
---
<p>When an excess charge (electron or hole) is added to a perfect crystal, there are several possible scenarios. In dielectric materials, the two extremes are a large polaron (wave function extending over many unit cells) and small polarons (wave function localised in a single unit cell; often on one atom). <a href="https://www.tandfonline.com/doi/abs/10.1080/00018736900101267">Austin and Mott</a> provided an essential overview of the topic in 1969.</p>

<p>I received several recent queries on this topic, so I thought it may be best to write a short post... I won't discuss large polarons here; a good place to start for those is the <a href="https://github.com/jarvist/PolaronMobility.jl">PolaronMobility.jl</a> package and related reading (e.g. the work of Dan Davies on <a href="https://pubs.acs.org/doi/10.1021/acs.jpclett.9b03398">rapid descriptors</a>). </p>

## Ingredients

<li>An electronic structure approach such as DFT that includes reliable atomic forces, e.g. GW theory won't necessarily help you here</li>
<li>An exchange-correlation functional without severe self-interaction errors, e.g. bare LDA/GGA are often insufficient to describe small polarons, hybrid functionals can be better (sensitive to % HF exchange), DFT+U can be okay (sensitive to choice of  U)</li>
<li>A crystal where small polarons form. Experimental evidence is often present from photoemission, EPR, conductivity measurements, etc.</li>

## Method

<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2020/08/screenshot-2020-08-04-at-08.55.35.png?w=1024" alt="" class="wp-image-1369" /></figure>

<p>I will take the example of Fe<sub>2</sub>O<sub>3</sub>, which we<a href="https://www.nature.com/articles/s41467-019-11767-9"> tackled</a> in collaboration with pump-push-probe spectroscopy led by Artem Bakulin. I give my approach for dealing with an excess electron, but many others exist, so don't take this as gospel.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:list {"ordered":true} --></p>
<ol>
<li><em><strong>Understand the perfect crystal.</strong> </em>In Fe<sub>2</sub>O<sub>3</sub>, the formal ions present are Fe(III), which have a d<sup>5</sup> electronic configuration in roughly octahedral coordination. These d<sup>5</sup> electrons prefer to be in a high spin configuration, and the lowest energy solution of the crystal is layer-by-layer anti-ferromagnetism along the hexagonal (0001) axis [<em>1</em>].</li>
<li><em><strong>Think about what can happen.</strong> </em>Adding an electron to Fe<sub>2</sub>O<sub>3</sub> is likely to reduce Fe from Fe(III) d<sup>5</sup> to Fe(II) d<sup>6</sup>. The d<sup>6</sup> configuration in an octahedral field can be high spin or low spin, so we'll have to check both solutions to find the lowest in total energy.</li>
<li><em><strong>Add an electron and check the result.</strong> </em>I take a supercell of my perfect crystal and change the electron count by 1. In this case, I find that the excess electron is delocalised across all equivalent Fe sites. I call this a "band electron" and it provides a useful reference energy. </li>
<li><strong><em>Break symmetry and relax the ions.</em> </strong>Often local optimisers fail to break the crystal symmetry spontaneously, so you have to give a "push" and then fall into the closet local minimum. There are several approaches to do this, such as: (a) replacing one Fe by another ion with appropriate radius, relaxing, and then substituting Fe back in; (b) adding small random atomic displacements around one Fe in the supercell; (c) a selective distortion around one Fe. Following geometry optimisation, they should all arrive at the same solution but may require different amounts of computer time. This is discussed in detail in the recent paper of <a href="https://pubs.acs.org/doi/pdf/10.1021/acs.jctc.0c00374">Pham and Deskins</a>.  Here the symmetry breaking and atomic relaxation serve to localise the excess electron and I call this a "small polaron".</li>
<li><em><strong>Map out the potential energy surface. </strong></em>So far we have two points, which is sufficient to define a polaron binding energy, but we can do more. The atomic geometries of the perfect crystal and the localised polaron can be used to define a configurational coordinate<em> Q</em> that maps between them (e.g. there are relevant scripts in <a href="https://github.com/WMD-group/CarrierCapture.jl">CarrierCapture.jl</a>). The challenge is over what range it is possible to obtain the band / polaron solutions. This requires playing with the wavefunction initialisation in whatever code you are using. A hands-on approach would be using full occupation control or constrained DFT. This is the direction we are actively working on, so I'll have to save that icing for later...</li>
</ol>

<p><em>[1] I incorrectly <a href="https://www.sciencedirect.com/science/article/pii/S0009261413011780">commented</a> that the layer-by-layer AFM description was only possible in the hexagonal setting of Fe<sub>2</sub>O<sub>3</sub>. Nicola Spaldin correctly pointed out to me a few weeks ago that it is possible in the primitive trigonal cell. </em></p>
<p><!-- /wp:paragraph --></p>
