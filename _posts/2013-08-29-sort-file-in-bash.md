---
layout: post
title: Sort File in Bash
---
This is a guide for sorting a given file in bash, which was given to me as a class assignment.

The file looks like this:
{% highlight bash %}
Mikaela Andersson       14   -  17 
David Fors               8  12  11 
Klas Hägglund            -  13   9 
Oskar Holmqvist          9  12  10 
Gustaf Karlsson         12  14  13 
Annica Rundgren         17  10  15 
Marcus Carlsson         12   9  12 
Johanna Rönnberg         9  15  13 
Josefine Carlson        18  14  16 
Magnus Ahlsten          13  18  14 
Jerker Leo               -   -  11 
Jan-Olof Kärrsgård       8  11  13 
Siv Tidblom             11   9  12 
Mats Karlzon            15  17  10 
Göran Johansson         16  19  15 
Britt Lidell             -  14   9 
Yngve Johanson          10  11   8 
Ingrid Lindgren          8  13  13 
Kjell Djurstedt         12  12  12 
Lennart Andersson        9  10  13 
Elisabeth Björklind     15  12  10 
{% endhighlight %}

And needs to looks like this:
{% highlight bash %}
50 Göran Johansson
48 Josefine Carlson
45 Magnus Ahlsten
42 Mats Karlzon
42 Annica Rundgren
39 Gustaf Karlsson
37 Johanna Rönnberg
37 Elisabeth Björklind 
36 Kjell Djurstedt
34 Ingrid Lindgren
33 Marcus Carlsson
32 Siv Tidblom
32 Lennart Andersson
32 Jan-Olof Kärrsgård
31 Oskar Holmqvist
31 Mikaela Andersson
31 David Fors
29 Yngve Johanson
23 Britt Lidell
22 Klas Hägglund
11 Jerker Leo
{% endhighlight %}

That is, we need to sum the numbers (a dash is treated like 0), place the score on the left side of the name and sort descending. We will do this in three steps.

<h2>Step 1 - Change dash to zero</h2>
<h2>Step 2 - The magic switcheroo</h2>
<h2>Step 3 - Sorting</h2>
{% highlight bash %}
sed ...|while read ...|sort -r
{% endhighlight %}

Since we want the lines in order, we'll append |sort -r to the command. 
<ol>
<li>The | ("pipe") means that we'll take the output from the command on the left side and give as input to the command on the right side.</li>
<li>sort will sort the lines in ascending order, since we wants it in descending instead, we will use the -r argument</li>
</ol>
