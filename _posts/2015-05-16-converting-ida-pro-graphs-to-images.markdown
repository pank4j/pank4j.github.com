---
author: pank4j
comments: true
date: 2015-05-16 15:23:25+00:00
layout: post
slug: converting-ida-pro-graphs-to-images
title: 'Converting IDA Pro graphs to images'
description: 'Converting IDA Pro graphs to images'
categories:
- IDA Pro
tags:
- IDA Pro
- graph
- control flow graph
- CFG
---

It's been a long time since my last post. I have been busy with work, and (thanks to IPR) could not post anything. This post is about a much needed feature in IDA Pro (specifically the wingraph32 utility), saving the graph as an image.

IDA Pro provides an interesting feature to view the disassembly of a function as a flowchart. The flowchart shows the function in the form of a control flow graph, where each node in the graph represents a basic block and each edge represents a control transfer. In many instances, one would like to export it as an image. However, the wingraph32 utility does not provide such an option.

<center>
![wingraph32](/public/images/wingraph32.png "A graph as shown by wingraph32")
</center>

The graph can be exported from IDA Pro (or from wingraoh32) as a .gdl (graph description language) file. The .gdl file can be converted into an image (or many other formats) using the [Graph::Easy](http://search.cpan.org/dist/Graph-Easy/bin/graph-easy) CPAN module.

First install the Graph::Easy CPAN module. It requires [Graphviz](http://www.graphviz.org/) to run, so install that as well.

{% highlight bash %}
cpan Graph::Easy
{% endhighlight %}

Then, convert the .gdl file to an image.

{% highlight bash %}
graph-easy --from gdl --input=graph.gdl --png --output=graph.png
{% endhighlight %}

This is how the final image of the above graph looks like.
<center>
![wingraph32](/public/images/graph.png "Graph converted by graph_easy")
</center>


