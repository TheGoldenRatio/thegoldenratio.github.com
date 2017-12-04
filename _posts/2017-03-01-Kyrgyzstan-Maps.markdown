---
layout: post
title: "Making A Better Kyrgyzstan Map"
date: 2017-03-12
comments: true
categories: [posts]
---
> You can find my code and files [here](https://github.com/TheGoldenRatio/maps-d3). Larger map [here](../img/kg100.svg). Refer to my previous post on mapping with D3 for more info. 

There aren't many great looking maps of Kyrgyzstan, which is a shame because it's a lovely country. For example, the United Nations Office for the Coordination of Humanitarian Affairs (UNOCHA) made this:

 ![ugly map](http://img.static.reliefweb.int/sites/reliefweb.int/files/styles/attachment-large/public/resources-pdf-previews/18652-E7BE45FFEA4484B2C125774200517C94-map.png?itok=y5KlQY-D)

It's enough to get the job done but it's aesthetically lacking and from 2009 so I wanted to make something more updated.

The first task was to find existing maps of the Kyrgyz oblasts in the proper digital files (.shp shapefiles or JSON). I kept on finding low quality maps (one represented Bishkek as a giant oval, which is seriously lazy) but I eventually found a good file and converted it to a JSON file.[^1]

The new JSON file has the oblast names and their geometries but it has no other information. No worries; we can pretty easily manually get this info because Kyrgyzstan only has 9 administrative regions (7 oblasts plus 2 independent cities)[^2] : 

Names 	  	  | Area (km^2)	| 	Population |  	 Pop. Density  |
 ------------ | -----------: | -----------: |-----------: |
Batken oblast |    17,048    |	480,700		|	28.19 |	    
Jalal-Abad oblast|	32,418	| 1,122,400	| 34.62 | 
Issyk-Kul oblast |	43,735	| 463,900	| 10.60 |
Naryn oblast | 44,160	| 274,500	| 6.21|
Osh oblast	| 28,934	| 1,228,400	| 42.45 |
Talas oblast |	13,406 |	247,200 |	18.43 |
Chui oblast |	19,895 |	870,300	| 43.74|
Bishkek city | 	170	| 937,400| 	5514.11|
Osh city| 	183	| 270,300	| 1477.04|

It's pretty obvious from inspecting the table that we're gonna have some issues. First, the population density of the two cities is 2 orders of magnitude greater than the other regions! That means that basically we'll have two bright little dots on an otherwise dark map if we want to accurately show the population density.[^3] 

Regardless, I proceeded to convert that table above into a JSON file and joined the information into our main ndJSON file.[^4]

This is what we get when we scale the population density threshold down to 100[^5] people/km^2[^6]:   

<br> ![kg100](../img/kg100.svg)  <br><br>

As expected, this doesn't look that much better at all because there's just not enough detail and resolution (I also should add a scale later and I suppose labels would help). But a couple improvements -- first, the border shapes are also slightly sharper. Secondly, this map made me realize that the UN map has completely misplaced the location of Osh (pretty sloppy)! The city needs to be moved south/ mirrored across the protruding bit of Uzbek territory.

I can't really say that I've made a better map (yet) so I'll have to do a part 2 to break down the country into smaller administrative regions, which will hopefully give us more information.

<br>

[^1]: ```` npm install -g shapefile```` <br> ````shp2json KGZ_adm1.shp -o kg.json ````
[^2]: [https://en.wikipedia.org/wiki/Regions_of_Kyrgyzstan](https://en.wikipedia.org/wiki/Regions_of_Kyrgyzstan)
[^3]: Also, with just 9 regions we're gonna have a pretty boring looking map even without the giant density disparity. Maps always look way cooler when they have lots of different areas, giving it high resolution.
[^4]: ```` ndjson-join \ ```` <br>  ````kg.ndjson oblast_info.ndjson \ ```` <br>  ````> kg-join.ndjson````  <br> <br> ````ndjson-map \```` <br> ```` 'd[0].properties = {density: d[1].density, name: d[1].name},  d[0]' \```` <br> ````  < kg-join.ndjson > kg-join-density.ndjson````
[^5]: if we scaled to 5000 to show the full range of population densities then the map would be uniformly dark with one bright spot (Bishkek) and one somewhat less dark spot (Osh). Even applying a log transform doesn't help much. Just the nature of our coarse data.
[^6]: ````  ndjson-map -r d3 \```` <br> ````'(d.properties.fill = d3.scaleSequential(d3.interpolateViridis).domain([0, 100])(d.properties.density), d)' \````  <br> ````< kg-join-density.ndjson \ ````<br> ````> kg-color.ndjson ```` <br><br>   ````geo2svg -n --stroke none -p 1 -w 960 -h 960 \ ```` <br> ````< kg-color.ndjson \ ```` <br> ```` > kg-color.svg```` <br><br>  
