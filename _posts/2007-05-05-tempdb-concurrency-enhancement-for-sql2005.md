---
layout: post
title: tempdb concurrency enhancement for sql2005
---

<div class="Section1">
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt">This tip is the pimpness. There are really no downsides and it can help a lot if you are having heavy contention with your tempdb.</span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"><a href="http://support.microsoft.com/kb/328551/en-us">http://support.microsoft.com/kb/328551/en-us</a></span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt">here’s a summary of what you do:</span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal" style="margin-left:.5in"><i><font size="2" face="Arial"><span style="font-size:10.0pt;font-style:italic">Increase the number of <b><span style="font-weight:bold">tempdb</span></b> data files to be at least equal to the number of processors. Also, create the files with equal sizing.</span></font></i></p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt">That’s pretty much it. Definitely read the article though as it has a ton of great technical information about why this works and what is going on in a situation where something like this is needed.</span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt"> </span></font>
        </p>
        <p class="MsoNormal">
            <font size="2" face="Arial"><span style="font-size:10.0pt">In general I would probably say if you *<b><span style="font-weight:bold">need</span></b>* this fix, you really *<b><span style="font-weight:bold">need</span></b>* to revisit why you are running a complete piece
                of crap application in a production environment, but that’s just me. =D</span>
            </font>
        </p>
    </div>
