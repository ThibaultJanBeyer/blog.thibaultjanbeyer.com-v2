---
layout: post
title: How to use CSS filter & CSS Animation on images
excerpt: Today I learned something very very very useful for web design
image: /assets/mona-lisa-manipulate-animate-img-color-css.png
image_alt: CSS filters tutorial
---

<p>Today I learned something very very very useful for web design:<br />
How to use CSS filters on images and how to animate them !</p>
<p>Why is it better than JavaScript, JQuery, animated .giff or else? Because it takes fewer resources and thus renders faster. Moreover JavaScript is some times deactivated on browsers.</p>
<div class="result monalisa"><img class="saturate" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="grayscale" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="contrast" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="brightness" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="blur" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="invert" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="sepia" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="huerotate" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /><img class="rss opacity" src="{{ site.baseurl }}/assets/Mona_Lisa_by_Leonardo_da_Vinci-201x300.jpg" alt="Mona Lisa" /></div>
<p>&nbsp;</p>
<p>Above is the results you can achieve. It is just like photoshopped, but better. And yes, you’ll learn how to use it in CSS animations as well.</p>
<h3>The HTML</h3>
<pre class="theme:sublime-text font:ubuntu-mono right-set:true lang:default decode:true">&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="saturate"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="grayscale"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="contrast"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="brightness"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="blur"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="invert"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="sepia"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="huerotate"&gt;
	&lt;img alt="Mona Lisa" src="Mona_Lisa_by_Leonardo_da_Vinci.jpg" class="rss opacity"&gt;</pre>
<h3>The CSS</h3>
<pre class="lang:css decode:true">.saturate {
    -webkit-filter: saturate(3);
    -moz-filter: saturate(3);
    -o-filter: saturate(3);
    filter: saturate(3);
}
.grayscale {
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    -o-filter: grayscale(100%);
    filter: grayscale(100%);
}
.contrast {
    -webkit-filter: contrast(160%);
    -moz-filter: contrast(160%);
    -o-filter: contrast(160%);
    filter: contrast(160%);
}
.brightness {
    -webkit-filter: brightness(0.25);
    -moz-filter: brightness(0.25);
    -o-filter: brightness(0.25);
    filter: brightness(0.25);
}
.blur {
    -webkit-filter: blur(3px);
    -moz-filter: blur(3px);
    -o-filter: blur(3px);
    filter: blur(3px);
}
.invert {
    -webkit-filter: invert(100%);
    -moz-filter: invert(100%);
    -o-filter: invert(100%);
    filter: invert(100%);
}
.sepia {
    -webkit-filter: sepia(100%);
    -moz-filter: sepia(100%);
    -o-filter: sepia(100%);
    filter: sepia(100%);
}
.huerotate {
    -webkit-filter: hue-rotate(180deg);
    -moz-filter: hue-rotate(180deg);
    -o-filter: hue-rotate(180deg);
    filter: hue-rotate(180deg);
}
.rss.opacity {
    -webkit-filter: opacity(50%);
    -moz-filter: opacity(50%);
    -o-filter: opacity(50%);
    filter: opacity(50%);
}</pre>
<p>Note that you’ll need vendor prefixes to get good browser support where -moz is firefox, -o opera and so on…</p>
<p>See <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/filter#Browser_compatibility" target="_blank">Browser compatibility</a> @MDN</p>
<p>I’m sure that you can figure many ways to use it! One i.e. would be to use it with CSS Animations like so:</p>
<div class="result">
<div id="background-color">Animating Background jpg IMG</div>
</div>
<pre class="lang:default decode:true ">&lt;div id="background-color"&gt;Animating Background jpg IMG	&lt;/div&gt;</pre>
<h3>The CSS</h3>
<pre class="lang:css decode:true">#background-color{
    background-image: url(asteroids-pattern.jpg);
    color: white;
    height: 200px;
    -webkit-animation: changeColor 5s infinite;
    -moz-animation:    changeColor 5s infinite;
    -o-animation:      changeColor 5s infinite;
    animation:         changeColor 5s infinite;
}
	
@-webkit-keyframes changeColor {
    0%   { -webkit-filter: hue-rotate(0deg); }
    100% { -webkit-filter: hue-rotate(360deg); }
}
@-moz-keyframes changeColor {
    0%   { -moz-filter: hue-rotate(0deg); }
    100% { -moz-filter: hue-rotate(360deg); }
}
@-o-keyframes changeColor {
    0%   { -o-filter: hue-rotate(0deg); }
    100% { -o-filter: hue-rotate(360deg); }
}
@keyframes changeColor {
    0%   { filter: hue-rotate(0deg); }
    100% { filter: hue-rotate(360deg); }
}</pre>
<p>So basically you start by setting the <i class="hilite">animation: changeColor 5s infinite;</i> on an element while changeColor will be the name of the animation. You can replace it with whatever you want. <i class="hilite">5s</i> is the animation speed or duration. By <i class="hilite">infinite</i> you set the repeat to infinite (write an integer instead to change it).</p>
<p>Then you create the animation with <i class="hilite">@keyframes</i> + the name of your animation. Here it was changeColor as set above. Lastly, you set key-frames. So 0% is the very beginning of the 5s animation while 100% is the end =&gt; 5 seconds.</p>
<p>CSS Animations are awesome and I use them a lot to create smooth and lightweight transitions! Now it’s your turn, I bet you can come up with something even better.</p>
