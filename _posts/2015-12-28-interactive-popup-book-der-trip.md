---
layout: post
title: Psychedelic popup book (with kaleidoscope, moire & illusory motion)
excerpt: Create and market a pop-up book with respective merchandise
image: /assets/Screen-Shot-2015-12-26-at-17.10.20-1024x558.png
image_alt: Dude taking LSD
---

<blockquote>Type: University<br />
Semester: 4 (2015)<br />
Skills: Pop up, Illustration, Psychedelics, Web, HTML, CSS &amp; JavaScript<br />
Co-Workers: Angelika Würz, Jessica Bensch, Salome Lorena Thiel</p></blockquote>
<h2>Task:</h2>
<p>– Create and market a pop-up book with respective merchandise.</p>
<h2>Our Solution:</h2>
<p>As we wanted to have sophisticated elements in our pop-up book, we decided to handcraft one about the experience of drugs. A lot of research went into how people react to which drug. Finally we chose to focus on a LSD trip and that is the Book we came up with:</p>
<p>[video width="1632" height="1080" mp4="http://blog.thibaultjanbeyer.com/wp-content/uploads/2015/12/Der-Trip.mp4"][/video]</p>
<p>As marketing Idea we came up with an interactive pwebsite pointing to the web-shop about the drug experience. <a href="http://kit.thibaultjanbeyer.com/trip/">Here is what I coded <i class="fa fa-rocket"></i></a>:</p>
<p><a href="http://kit.thibaultjanbeyer.com/trip/"><img class="alignnone wp-image-2100 size-large" src="{{ site.baseurl }}/assets/Screen-Shot-2015-12-26-at-17.10.20-1024x558.png" alt="psychedelic website lsd trip" width="809" height="441" /></a></p>
<p>and the <a href="http://kit.thibaultjanbeyer.com/trip/shop/">Webshop <i class="fa fa-rocket"></i></a> (<strong>with no functionality</strong> since it is just a concept mock up):</p>
<p><a href="http://kit.thibaultjanbeyer.com/trip/shop/"><img class="alignnone wp-image-2101 size-large" src="{{ site.baseurl }}/assets/The-Trip-Shop-&#8211;-just-another-psychedelic-shop-2015-12-26-17-12-51-718x1024.png" alt="Trip Shop psychedelic shop" width="718" height="1024" /></a></p>
<h2>Progress:</h2>
<p>Since we wanted to come up with fancy special effects that work on paper, beside complex paperwork and illustrations, I decided to dig deep into optical illusions and taught myself to understand and create 3 kind of optical illusions:</p>
<ol>
<li>Kaleidoscopics</li>
<li>The Moiré Effect</li>
<li>Self-Animating images (illusory motion)</li>
</ol>
<h3>1. Kaleidoscopics:</h3>
<p><img class="alignnone wp-image-2109 size-medium" src="{{ site.baseurl }}/assets/drugggz-400x400.png" alt="kaleidoscopic img kaleidoscope" width="400" height="400" /> <img class="alignnone size-medium wp-image-2105" src="{{ site.baseurl }}/assets/red3-400x400.png" alt="kaleidoscopic img kaleidoscope" width="400" height="400" /></p>
<p><img class="alignnone wp-image-2107 size-full" src="{{ site.baseurl }}/assets/susi.png" alt="kaleidoscopic img kaleidoscope" width="1000" height="1000" /></p>
<p><img class="alignnone size-medium wp-image-2111" src="{{ site.baseurl }}/assets/psy-solo-s-400x364.png" alt="kaleidoscopic img kaleidoscope" width="400" height="364" /> <img class="alignnone size-medium wp-image-2110" src="{{ site.baseurl }}/assets/feather_solo_s-400x342.png" alt="kaleidoscopic img kaleidoscope" width="400" height="342" /></p>
<p><img class="alignnone size-large wp-image-2108" src="{{ site.baseurl }}/assets/Aufmacherseiten_2-1024x724.png" alt="kaleidoscopic img kaleidoscope" width="809" height="572" /></p>
<p>These are fairly simple to understand and reproduce. A kaleidoscope is just a cylinder with mirrors reflecting an image. While I was building my own real kaleidoscope to understand how it works, I figured out that the standard kaleidoscope has six mirrors to create hexagonal refraction. With this knowledge in mind you can now build your own pattern: if you cut out a triangle from an image and glue copies of that triangle to the sides of the first you get a hexagon which already looks pretty weird. Now glue several hexagons together for a kaleidoscopic pattern. See how I did it:</p>
<p><img class="alignnone size-large wp-image-2102" src="{{ site.baseurl }}/assets/how-to-make-a-kaleidoscopic-pattern-1024x675.png" alt="how to kaleidoscopic pattern illustrator" width="809" height="533" /></p>
<p>Fortunately, <strong>I wrote an illustrator script</strong> that will make all that heavy work for you, it's available on Github: <a href="https://github.com/ThibaultJanBeyer/kaleidoscope-pattern-now.js" target="_blank">kaleidoscope-pattern-now.js <i class="fa fa-github-alt"></i></a> check it out:</p>
<p><a href="https://github.com/ThibaultJanBeyer/kaleidoscope-pattern-now.js" rel="attachment wp-att-2129"><img class="alignnone wp-image-2129 size-full" src="{{ site.baseurl }}/assets/kaleidoscope-pattern-now_part1.gif" alt="kaleidoscope-pattern-now how to part1" width="800" height="500" /></a></p>
<p><a href="https://github.com/ThibaultJanBeyer/kaleidoscope-pattern-now.js"><img class="alignnone wp-image-2130 size-full" src="{{ site.baseurl }}/assets/kaleidoscope-pattern-now_part2.gif" alt="kaleidoscope-pattern-now how to part2" width="800" height="500" /></a></p>
<h3>2. The Moiré Effect:</h3>
<p><img class="alignnone size-full wp-image-2128" src="{{ site.baseurl }}/assets/muster_boden.png" alt="moire effect" width="900" height="658" /></p>
<p>The Moiré Effect is a simple brain-hack – you might have heard of it as something you "don't want" and photographs hate it but I'm sure that you'll love this variant. When used right the well-known Moiré Effect is an optical illusion that let's you kind of animate you paper. Here is an example video of cool Moiré Effects:</p>
<p>[video width="1280" height="720" mp4="http://blog.thibaultjanbeyer.com/wp-content/uploads/2015/12/Effet-moiré-animation-YouTube.mp4"][/video]</p>
<p>This as well is not that difficult to create. Print black bars with a distance of x from one bar to the next. Now let's say you have an animation of 3 Frames (thus 3 images) you'll have to cut the picture. Do it so that there is only one image of width x remaining for that frame. Repeat for frame 2 but x pixels more to the left. Repeat for the 3rd frame. Well it is easier to make than to explain. I've found <a href="http://www.instructables.com/id/2D-Moire-Slit-Animation/" target="_blank">a handy tutorial explaining</a> it. I bought <a href="http://www.amazon.de/Visual-Illusions-Motion-Moire-Screens/dp/0486295710/ref=sr_1_1?ie=UTF8&amp;qid=1431450698&amp;sr=8-1&amp;keywords=Visual+Illusions+in+Motion+with+Moir%C3%A9+Screens" target="_blank">this nice book</a> which is a collection of Moiré effects and this <a href="http://www.amazon.de/Theory-Moir%C3%A9-Phenomenon-Periodic-Computational-ebook/dp/B008BBQYIM/ref=sr_1_1?ie=UTF8&amp;qid=1431450673&amp;sr=8-1&amp;keywords=The+Theory+of+the+Moir%C3%A9+Phenomenon" target="_blank">book explaining the effect</a>. The same applies to round objects (as used in the toilet in our pop-up book):</p>
<p><img class="alignnone size-large wp-image-2120" src="{{ site.baseurl }}/assets/round-2-1024x724.png" alt="round moire effect" width="809" height="572" /></p>
<h3>3. Self-Animating images (illusory motion):</h3>
<p><img class="alignnone wp-image-2118 size-full" src="{{ site.baseurl }}/assets/moving-stuff3.png" alt="moving image illusory motion" width="1305" height="1035" /></p>
<p><img class="alignnone size-full wp-image-2125" src="{{ site.baseurl }}/assets/moving-stuff4.png" alt="moving image illusory motion" width="842" height="595" /></p>
<p><img class="alignnone size-full wp-image-2124" src="{{ site.baseurl }}/assets/moving-stuff3-1.png" alt="moving image illusory motion" width="842" height="474" /></p>
<p><img class="alignnone size-full wp-image-2119" src="{{ site.baseurl }}/assets/smile_s.png" alt="moving image illusory motion" width="1200" height="800" /></p>
<p><img class="alignnone size-large wp-image-2116" src="{{ site.baseurl }}/assets/central4-1024x724.png" alt="moving image illusory motion" width="809" height="572" /></p>
<p><img class="alignnone size-large wp-image-2113" src="{{ site.baseurl }}/assets/central-1024x724.png" alt="moving image illusory motion" width="809" height="572" /></p>
<p>This, however, is very complex. I won't be able to explain it in detail nor show you how I did it in one picture or gif. Somehow not everyone can perceive this kind of illusions (a small amount of human population is immune), but most will. Fortunately, I found some information online including two paper describing it pretty well. Here is all documentation I read:</p>
<ul>
<li>&gt; <a href="http://www.ucl.ac.uk/~ucbpmor/docs/case_study3_mor_web.pdf" target="_blank">Paper 1</a></li>
<li>&gt; <a href="http://graphics.csie.ncku.edu.tw/SAI/c38-f38_2-a276-paperfinal.pdf" target="_blank">Paper 2</a></li>
<li><a href="https://en.wikipedia.org/wiki/Optical_flow" target="_blank">Optical Flow – wiki</a></li>
<li><a href="https://en.wikipedia.org/wiki/Op_art" target="_blank">Op Art – wiki</a></li>
<li><a href="http://graphicdesign.stackexchange.com/questions/26351/how-are-patterns-that-look-like-theyre-moving-created" target="_blank">How to question</a></li>
<li><a href="http://www.livescience.com/33664-amazing-optical-illusions-work.html" target="_blank">Some illusions</a></li>
<li><a href="http://michaelbach.de/ot/" target="_blank">Large collection</a></li>
<li><a href="http://beta.nodebox.net/documentation/tutorial/animation.html" target="_blank">Basic Text Animation</a></li>
</ul>
<p>Here is a basic info about the direction depending on the type. Without any explanation why. It's always going from Dark, White, Lighter to Black. Here is a combination of the three most effective combinations with the best colors. The aim is to create a high contrast. Also the last Type (c) is the strongest:</p>
<p><img class="alignnone size-full wp-image-2127" src="{{ site.baseurl }}/assets/Screen-Shot-2015-12-27-at-22.02.34.png" alt="illusory motion" width="848" height="314" /></p>
<p>Again, perception of illusory movement is subjective and some are even immune to it.</p>
<p><img class="alignnone size-full wp-image-2117" src="{{ site.baseurl }}/assets/central5.png" alt="moving image illusory motion" width="4000" height="3977" /></p>
<p><img class="alignnone size-full wp-image-2122" src="{{ site.baseurl }}/assets/error.png" alt="error" width="1191" height="400" /></p>
<p>The last one is broken by the way, for the case you where wondering "oooh I see no animation on that one".</p>
<p>Now you got new knowledge in your hands, use it wisely ;)</p>
<p>[video width="1080" height="1080" mp4="http://blog.thibaultjanbeyer.com/wp-content/uploads/2015/12/sog_3_c.mp4"][/video]</p>
