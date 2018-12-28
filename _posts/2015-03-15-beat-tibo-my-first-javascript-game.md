---
layout: post
title: Beat Tibo – my first Javascript Game.
excerpt: my first Javascript Game ever
image: /assets/Screen-Shot-2014-04-06-at-09.24.42.png
image_alt: My first javascript game ever
---

<blockquote>Type: Private/Free<br />
Skills: JavaScript, HTML, CSS</p>
<p>Year: 2010</p></blockquote>
<p>So yeah as Beat Tibo is my first JavaScript game ever it is pretty noobish with all the <em>confirm()</em> and <em>prompt()</em> and especially the <em>setTimeout()</em> that might seem a bit spammy but hey, first javascript game ever.</p>
<p>Here's the code for you to understand, it is pretty simple:</p>
<pre class="theme:sublime-text font:ubuntu-mono font-size:14 line-height:18 right-set:true tab-convert:true lang:default decode:true">&lt;script&gt;
    function story(){
        var display = document.getElementById("display");
        var button = document.getElementById("button");
        var ready = confirm("Are you ready mah' nigg' ?");
        var age = prompt("How old are you ? (18+ game bro')");
        if ((ready === true) &amp;amp; (age &gt;= 18)){
            button.style.visibility="hidden";
            display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;All fine dude! &lt;span style="color: #deb887;"&gt;Ready &amp;amp; "+age+"&lt;/span&gt;.";
            setTimeout(function(){
                display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;Now read &lt;span style="color: #deb887;"&gt;carefully&lt;/span&gt;:";
                setTimeout(function(){
                    display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;&lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt; was hanging out at the bus stop.&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;There was a sale on and he needed new shoes.&lt;span style="color: grey;"&gt;3 | &lt;/span&gt;You've never really liked &lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt;.&lt;span style="color: grey;"&gt;4 | &lt;/span&gt;You walk up to him..";
                    setTimeout(function(){
                        display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;&lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt; glares at you...";
                        setTimeout(function(){
                            var userAnswer = prompt("Are you feeling lucky, punk?");
                            if(userAnswer === "yes"){
                                display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;&lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt; hits you very hard.&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;It's &lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt; and you're you! Of course &lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt; wins!";
                            } else {
                                display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;You did not say "yes".&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;Good choice!&lt;span style="color: grey;"&gt;3 | &lt;/span&gt;You are a winner of not getting beaten up by &lt;span style="color: #deb887;"&gt;Tibo&lt;/span&gt;.";
                                }
                            setTimeout(function(){
                                var feedback = prompt("Please rate my game from 1-10. Where 10 is the highest rating.");
                                if(feedback &gt; 8){
                                    display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;This is just the beginning of my game empire.&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;&lt;a href="http://creativemate.net/" target="_blank"&gt;Stay tuned&lt;/a&gt; for more!";
                                    button.style.visibility="visible";
                                }else{
                                    display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;What?!&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;The nerve!&lt;span style="color: grey;"&gt;3 | &lt;/span&gt;Just you wait!&lt;span style="color: grey;"&gt;4 | &lt;/span&gt;Ok ok ok ok ok I got it..&lt;span style="color: grey;"&gt;5 | &lt;/span&gt;I'll train harder to come up with some real shit.&lt;span style="color: grey;"&gt;6 | &lt;/span&gt;So you should definitely &lt;a href="http://creativemate.net/" target="_blank"&gt;stay tuned&lt;/a&gt;!";
                                    button.style.visibility="visible";
                                }
                            },5000);
                        },2000);
                    },10000);
                },3000);
            },3000);
        }else{
            display.innerHTML = "&lt;span style="color: grey;"&gt;1 | &lt;/span&gt;Guy something is wrong here.&lt;span style="color: grey;"&gt;2 | &lt;/span&gt;&lt;span style="color: #deb887;"&gt;Ready = "+ready+"&lt;/span&gt; and &lt;span style="color: #deb887;"&gt;Age = "+age+"&lt;/span&gt;?&lt;span style="color: grey;"&gt;3 | &lt;/span&gt;Ok man. You can't play. So get out of here creep!";
        }
    };
&lt;/script&gt;</pre>
<p>&nbsp;</p>
<p>Yah so the code is basically like: question - if true - text - wait - then next question - if else - other text - wait - other question.</p>
<p>you can check out the <em>html</em> and <em>css</em> on page. <i class="hilite">At least the design rocks</i> ;)</p>
<p>Ok enough talking <a href="https://thibaultjanbeyer.github.io/thibaultjanbeyer.com-v7/work/lab/javascript/bt" target="_blank">go to the game</a>.</p>
