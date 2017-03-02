---
layout: post
title: "Utah Choropleth"
date: 2017-02-27
comments: true
categories: [posts]
---

>Saw a really great D3 map [tutorial](https://medium.com/@mbostock/command-line-cartography-part-1-897aa8f8ca2c#.7h8t0t2ze) by Mike Bostock, the creator of D3, and made these by following along. Higher resolution images [here](/images/ut-tracts-sqrt.svg) and [here](/images/ut-tracts-log.svg).

The following are choropleths[^1] which display the population densities of Utah (my home state, kind of). The data is taken from the Census Bureau's API and using d3-geo, GeoJSON, and TopoJSON, we're able to map out a state's geographic information in JSON form, then combine the Census Bureau info into the JSON.     

This first image shows how concentrated Utah's population of three million really is[^2]. Starting from the top of the state, the very first small dot is Logan (pop. ~49,000). An almost microscopic green pinpoint under it is Brigham City (pop. ~18,000). 

Then we reach the Wasatch Front, which is roughly composed of three blobs (the Salt Lake City-Ogden-Provo Combined Statistical Area). The northern-most blob is the Ogden-Clearfield area (pop. ~600,000) and Layton (~70,000). The middle blob is the Salt Lake City metropolitan area (~ 1.2 million).  The lowest blob is the Provo-Orem metropolitan area (pop. ~590,000), which roughly outlines eastern border of Utah Lake. 

There are a few more areas that we can pick out. At the southwestern corner of the state is St. George (met. pop. ~160,000) and to the northeast right above it is a tiny dot (Cedar City, pop. ~30,000). Vernal (~10,800), Price (~8,700), and Moab (almost invisible, pop. ~5,000) make a triangle in the eastern-central portion of the state. Park City (~8,000) and Heber (~13,000) are to the east of Salt Lake City.


![utah-sqrt](/images/ut-tracts-sqrt.svg)


This next choropleth uses a log scale in order to bring out some of the less populous areas. Though we lose some of the resolution present in the previous graph, I like how it helps bring out some of the smaller cities that are barely visible in the previous graph[^4]. And because this graph is more colorful and I like the green/yellow color, I think I slightly prefer this graph even though it is a less accurate in showing population concentration and distribution.

![utah-log](/images/ut-tracts-log.svg)  


<br>
Footnotes:
<br>  

[^1]: The word choropleth is derived from the Greek words χῶρος, which means "area/region" and πλῆθος, meaning "multitude."
[^2]: According to Wikipedia, 80% of the state's population is along the Wasatch Front and this graph seems to show that nicely. Utah is the 13th largest state by area but the 40th in terms of population density.
[^4]: In particular:  Moab (~5,000), Duschene (~1,800), Delta (~3,500), Monroe (~2,256). All pretty interesting places, particularly Moab and Delta. Moab is close to Arches and Canyonlands National Parks so it has lots of outdoor tourism. The Topaz War Relocation Center is right outside Delta. It's where Japanese-Americans were incarcerated in their own country after FDR signed an executive order 75 years and a week ago on Feb. 17, 1942. 
