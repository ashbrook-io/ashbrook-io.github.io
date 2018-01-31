---


---

<p>{% gist 840230f33e81bdbabd337a498f9ae255 %}</p>
<p>Edit:</p>
<p>It’s worth noting I found this set of code to manually split the jobs up. I ran it with a cap higher than the total number of machines and it did finish faster.</p>
<pre class=" language-powershell"><code class="prism  language-powershell"><span class="token comment">#baseline comparison of localhost</span>
<span class="token function">PS</span> C:\&gt;<span class="token function">Measure-Command</span> <span class="token punctuation">{</span><span class="token variable">$Results</span> = <span class="token function">Test-Connection</span> localhost<span class="token punctuation">}</span>


Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 3
Milliseconds      : 109
Ticks             : 31091556
TotalDays         : 3<span class="token punctuation">.</span>59855972222222E<span class="token operator">-</span>05
TotalHours        : 0<span class="token punctuation">.</span>000863654333333333
TotalMinutes      : 0<span class="token punctuation">.</span>05181926
TotalSeconds      : 3<span class="token punctuation">.</span>1091556
TotalMilliseconds : 3109<span class="token punctuation">.</span>1556


<span class="token comment">#just doing a normal list pass in </span>
<span class="token function">PS</span> C:\&gt; <span class="token function">Measure-Command</span> <span class="token punctuation">{</span> `
&gt;&gt; <span class="token variable">$Results</span> = <span class="token function">Test-Connection</span> <span class="token operator">-</span>ttl 5 <span class="token operator">-</span>Count 1 <span class="token operator">-</span>ErrorAction SilentlyContinue <span class="token operator">-</span>ComputerName <span class="token variable">$servers</span><span class="token punctuation">}</span>


Days              : 0
Hours             : 0
Minutes           : 2
Seconds           : 57
Milliseconds      : 906
Ticks             : 1779068137
TotalDays         : 0<span class="token punctuation">.</span>0020591066400463
TotalHours        : 0<span class="token punctuation">.</span>0494185593611111
TotalMinutes      : 2<span class="token punctuation">.</span>96511356166667
TotalSeconds      : 177<span class="token punctuation">.</span>9068137
TotalMilliseconds : 177906<span class="token punctuation">.</span>8137


<span class="token comment">#what I actually did when I was working, takes less than half the high time of the normal list</span>
<span class="token function">PS</span> C:\&gt; <span class="token function">Measure-Command</span> <span class="token punctuation">{</span> `
&gt;&gt; <span class="token variable">$job</span> = <span class="token function">Test-Connection</span> <span class="token operator">-</span>ttl 5 <span class="token operator">-</span>Count 1 <span class="token operator">-</span>ErrorAction SilentlyContinue <span class="token operator">-</span>ComputerName <span class="token variable">$servers</span> <span class="token operator">-</span>AsJob
&gt;&gt; <span class="token variable">$null</span> = <span class="token function">Wait-Job</span> <span class="token variable">$job</span>
&gt;&gt; <span class="token variable">$Results</span> = <span class="token function">Receive-Job</span> <span class="token variable">$job</span> <span class="token punctuation">}</span>


Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 21
Milliseconds      : 98
Ticks             : 210987558
TotalDays         : 0<span class="token punctuation">.</span>0002441985625
TotalHours        : 0<span class="token punctuation">.</span>0058607655
TotalMinutes      : 0<span class="token punctuation">.</span>35164593
TotalSeconds      : 21<span class="token punctuation">.</span>0987558
TotalMilliseconds : 21098<span class="token punctuation">.</span>7558


<span class="token comment">#what i found online when i looked around as i was curious if asjob was parallel. it's about half of the faster one</span>
<span class="token comment">#inspired by this: https://stackoverflow.com/questions/8781666/run-n-parallel-jobs-in-powershell</span>
<span class="token function">PS</span> C:\&gt; <span class="token function">Measure-Command</span> <span class="token punctuation">{</span>
&gt;&gt; <span class="token variable">$List</span> = <span class="token variable">$servers</span>
&gt;&gt; <span class="token variable">$Jobs</span> = 100
&gt;&gt;
&gt;&gt; <span class="token keyword">Foreach</span> <span class="token punctuation">(</span><span class="token variable">$PC</span> in <span class="token variable">$List</span><span class="token punctuation">)</span>
&gt;&gt; <span class="token punctuation">{</span>
&gt;&gt; <span class="token keyword">Do</span>
&gt;&gt;     <span class="token punctuation">{</span>
&gt;&gt;     <span class="token variable">$Job</span> = <span class="token punctuation">(</span><span class="token function">Get-Job</span> <span class="token operator">-</span>State Running <span class="token punctuation">|</span> <span class="token function">measure</span><span class="token punctuation">)</span><span class="token punctuation">.</span>count
&gt;&gt;     <span class="token punctuation">}</span> <span class="token keyword">Until</span> <span class="token punctuation">(</span><span class="token variable">$Job</span> <span class="token operator">-le</span> <span class="token variable">$Jobs</span><span class="token punctuation">)</span>
&gt;&gt;
&gt;&gt; <span class="token function">Start-Job</span> <span class="token operator">-</span>Name <span class="token variable">$PC</span> <span class="token operator">-</span>ScriptBlock <span class="token punctuation">{</span> <span class="token function">Test-Connection</span> <span class="token operator">-</span>ttl 5 <span class="token operator">-</span>Count 1 <span class="token operator">-</span>ErrorAction SilentlyContinue <span class="token operator">-</span>ComputerName <span class="token variable">$PC</span> <span class="token punctuation">}</span>
&gt;&gt; <span class="token function">Get-Job</span> <span class="token operator">-</span>State Completed <span class="token punctuation">|</span> <span class="token function">Remove-Job</span>
&gt;&gt; <span class="token punctuation">}</span>
&gt;&gt;
&gt;&gt; <span class="token function">Wait-Job</span> <span class="token operator">-</span>State Running
&gt;&gt; <span class="token function">Get-Job</span> <span class="token operator">-</span>State Completed <span class="token punctuation">|</span> <span class="token function">Remove-Job</span>
&gt;&gt; <span class="token variable">$Results</span> = <span class="token function">Get-Job</span>
&gt;&gt; <span class="token punctuation">}</span>


Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 13
Milliseconds      : 299
Ticks             : 132993910
TotalDays         : 0<span class="token punctuation">.</span>000153928136574074
TotalHours        : 0<span class="token punctuation">.</span>00369427527777778
TotalMinutes      : 0<span class="token punctuation">.</span>221656516666667
TotalSeconds      : 13<span class="token punctuation">.</span>299391
TotalMilliseconds : 13299<span class="token punctuation">.</span>391
</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

