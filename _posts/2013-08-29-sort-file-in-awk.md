---
layout: post
title: Sort File in Awk
---
<p class="meta">29 August 2013</p>

This is a guide for sorting a given file in awk, which was given to me as a class assignment. Note that I will use gawk (GNU awk), so if you try the code and it doesn't work, try changing awk for gawk.

The file looks like this:
<pre class="terminal"><code>
Mikaela Andersson       14   -  17 
David Fors               8  12  11 
Klas H&auml;gglund            -  13   9 
Oskar Holmqvist          9  12  10 
Gustaf Karlsson         12  14  13 
Annica Rundgren         17  10  15 
Marcus Carlsson         12   9  12 
Johanna R&ouml;nnberg         9  15  13 
Josefine Carlson        18  14  16 
Magnus Ahlsten          13  18  14 
Jerker Leo               -   -  11 
Jan-Olof K&auml;rrsg&aring;rd       8  11  13 
Siv Tidblom             11   9  12 
Mats Karlzon            15  17  10 
G&ouml;ran Johansson         16  19  15 
Britt Lidell             -  14   9 
Yngve Johanson          10  11   8 
Ingrid Lindgren          8  13  13 
Kjell Djurstedt         12  12  12 
Lennart Andersson        9  10  13 
Elisabeth Bj&ouml;rklind     15  12  10 
</code></pre>

And needs to looks like this:
<pre class="terminal"><code>
50 G&ouml;ran Johansson
48 Josefine Carlson
45 Magnus Ahlsten
42 Mats Karlzon
42 Annica Rundgren
39 Gustaf Karlsson
37 Johanna R&ouml;nnberg
37 Elisabeth Bj&ouml;rklind 
36 Kjell Djurstedt
34 Ingrid Lindgren
33 Marcus Carlsson
32 Siv Tidblom
32 Lennart Andersson
32 Jan-Olof K&auml;rrsg&aring;rd
31 Oskar Holmqvist
31 Mikaela Andersson
31 David Fors
29 Yngve Johanson
23 Britt Lidell
22 Klas H&auml;gglund
11 Jerker Leo
</code></pre>

That is, we need to sum the numbers (a dash is treated like 0), place the score on the right side of the name and sort descending. We will do this in two steps.

<h2>Step 1 - The magic switcheroo</h2>
{% highlight bash %}
awk '$0=$3+$4+$5FS$1FS$2' resultat.txt
{% endhighlight %}

<pre class="terminal"><code>
awk '$0=$3+$4+$5FS$1FS$2' resultat.txt
</code></pre>

The command work like this:
<ol>
<li>Awk will save the first line (Mikaela Andersson 14 - 17) to $0</li>
<li>Every word in $0 is split into separate variables, e.g. Mikaela will be saved to $1 and Andersson will be saved to $2</li>
<li>Through our expression we will change $0 to
  <ol>
  <li>Sum of the numbers (through $3 + $4 + $5 FS)</li>
  <li>First name Last name (through $1 FS $2)</li>
  Where FS means "space"
  </ol></li>
<li>Awk prints out $0</li>
<li>Repeat steps 1-4 for each line in file</li>
</ol>

<h2>Step 2 - Sorting</h2>
{% highlight bash %}
awk '$0=$3+$4+$5FS$1FS$2' resultat.txt|sort -r
{% endhighlight %}

Since we want the lines in order, we'll append |sort -r to the command. 
<ol>
<li>The | ("pipe") means that we'll take the output from the command on the left side and give as input to the command on the right side.</li>
<li>sort will sort the lines in ascending order, since we wants it in descending instead, we will use the -r argument</li>
</ol>
