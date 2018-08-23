---
layout: post
title: "Social Network Graph"
date: 2017-04-13
categories: [data]
excerpt: a D3 visualization built for the University of Central Asia
tags: D3 visualization
---
> The visualization probably doesn't look very good on a mobile device. [Here](../d3/net.html) it is by itself. Drag and hover to interact with the visualization.

<style>
.viz {
    font-size: 20px;
    padding-top:50px;
}
.links line {
stroke: #999;
stroke-opacity: 0.6;
}

.nodes circle {
stroke: #fff;
stroke-width: 1.5px;
}

text{
font-size: 10pt;
}
</style>

<h1 class="viz" >Yielded Students: Preadmit Social Network </h1>
<svg width="800" height="600"></svg>
<script src="../d3/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.24.0/d3-legend.min.js"></script>
<script src="https://raw.githubusercontent.com/susielu/d3-legend/master/d3-legend.min.js"></script>
<script>
var svg = d3.select("svg"),
width = +svg.attr("width"),
height = +svg.attr("height");

var color = d3.scaleOrdinal(d3.schemeCategory20);

var simulation = d3.forceSimulation()
.force("link", d3.forceLink().id(function(d) { return d.id; }))
.force("charge", d3.forceManyBody().distanceMax(150))
.force("center", d3.forceCenter(width / 2.5, height / 2));

var ordinal = d3.scaleOrdinal()
.domain(["Kyrgyz", "Tajik (GBAO)", "Tajik (non-GBAO)", "Kazakh", "Pakistani", "Afghan"])
.range([ "rgb(174, 199, 232)", "rgb(255, 187, 120)", "rgb(44, 160, 44)", "rgb(152, 223, 138)", "rgb(255, 127, 14)", "rgb(31, 119, 180)"]);

var svg = d3.select("svg");

svg.append("g")
.attr("class", "legendOrdinal")
.attr("transform", "translate(20,20)");

var legendOrdinal = d3.legendColor()
//d3 symbol creates a path-string, for example
//"M0,-8.059274488676564L9.306048591020996,
//8.059274488676564 -9.306048591020996,8.059274488676564Z"
.shape("path", d3.symbol().type(d3.symbolCircle).size(100)())
.shapePadding(10)
//use cellFilter to hide the "e" cell
.cellFilter(function(d){ return d.label !== "e" })
.scale(ordinal);

svg.select(".legendOrdinal")
.call(legendOrdinal);


d3.json("../d3/students.json", function(error, graph) {
if (error) throw error;

var link = svg.append("g")
.attr("class", "links")
.selectAll("line")
.data(graph.links)
.enter().append("line")
.attr("stroke-width", function(d) { return Math.sqrt(d.value); });

var node = svg.append("g")
.attr("class", "nodes")
.selectAll("circle")
.data(graph.nodes)
.enter().append("circle")
.attr("r", 5)
.attr("fill", function(d) { return color(d.group); })
.call(d3.drag()
.on("start", dragstarted)
.on("drag", dragged)
.on("end", dragended));

node.append("title")
.text(function(d) { return d.id; });

simulation
.nodes(graph.nodes)
.on("tick", ticked);

simulation.force("link")
.links(graph.links);

function ticked() {
link
.attr("x1", function(d) { return d.source.x; })
.attr("y1", function(d) { return d.source.y; })
.attr("x2", function(d) { return d.target.x; })
.attr("y2", function(d) { return d.target.y; });

node
.attr("cx", function(d) { return d.x; })
.attr("cy", function(d) { return d.y; });
}

});

function dragstarted(d) {
if (!d3.event.active) simulation.alphaTarget(0.2).restart();
d.fx = d.x;
d.fy = d.y;
}

function dragged(d) {
d.fx = d3.event.x;
d.fy = d3.event.y;
}

function dragended(d) {
if (!d3.event.active) simulation.alphaTarget(0);
d.fx = null;
d.fy = null;
}

</script>


I recently finished the D3 social network graph that I started a couple months ago. The results are interesting, though messy. I didn't do any advanced graph calculations so the following is all just based on visual inspection.

Some basic insights:

- Between the 67 nodes, we had over 750 edges
    - 1 node = individual person, 1 edge = relationship between two nodes.  

- The most isolated nodes had 0 links/edges (*i.e. didn't know anybody at the university prior to attending*)  

- The most connected node had more than 30 links.
    - These were students that attended the university's summer camp program.

- That's an average of 11 edges per node (*i.e. the mean number of connections per person was 11*).

- 19% of the nodes aren't connected to the main group (*i.e. no nth degree connections to the largest group in the graph*).  

- In general, the individuals who had the least connections, still have fewer connections. While this seems obvious, I feel like it's still worth noting that it's (generally) easier to add linkages than to break them, so those with more initial connections maintain their more-connected status.

- By far the most connected group was the Tajik GBAO students. This is not surprising as the region seems to be more tight-knit, geographically concentrated and isolated, and religiously/ethnically homogenous than the other places.

- Kyrgyz, non-GBAO Tajiks had average connectedness (due to larger geographic spread).

- Pakistani, Kazakh, and Afghan students had low connectedness (even larger geographic spread).

This graph is interesting because I highly doubt there are many other universities that have this level of interconnectedness prior to admissions. At Yale, I knew four people prior to attending and that seemed pretty normal; a lot of my classmates knew zero to two people? Of course there were people who went to more famous high schools and probably knew 30+ people but that wasn't very common at all.

In conclusion, I hadn't realized how generally connected the students were but it does seem to explain some of the interesting social dynamics, post hoc.
