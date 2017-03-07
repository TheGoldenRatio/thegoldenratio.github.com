---
layout: post
title: "Making Better Kyrgyzstan Maps"
date: 2017-03-01
comments: true
categories: [posts]
---
> You can find my code and files [here](github.com/TheGoldenRatio). Higher res map [here](). 
Thanks to Mike Bostock's excellent article on mapping with D3. 

There aren't many great looking maps of Kyrgyzstan, which is a shame because it's a lovely country. For example, the United Nations Office for the Coordination of Humanitarian Affairs (UNOCHA) made this:

 ![ugly map](http://img.static.reliefweb.int/sites/reliefweb.int/files/styles/attachment-large/public/resources-pdf-previews/18652-E7BE45FFEA4484B2C125774200517C94-map.png?itok=y5KlQY-D)

It's enough to get the job done but it's aesthetically lacking and from 2009 so I wanted to make something better.

The first task was to find existing maps of the Kyrgyz oblasts in the proper digital files (.shp shapefiles or JSON). I kept on finding low quality maps (one represented Bishkek as a giant oval, which is seriously lazy) but I eventually found a good file and converted it to a JSON file.[^1]

The new JSON file has the oblast names and their geometries but it has no other information. No worries; we can pretty easily manually get this info even without API calls because Kyrgyzstan only has 9 administrative regions (7 oblasts plus 2 independent cities)[^2] : 

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

It's pretty obvious from inspecting the table that we're gonna have some issues. First, the population density of the two cities is 2 orders of magnitude greater than the other regions!! That means that basically we'll have two bright little dots on an otherwise dark map if we want to accurately show the population density.[^3] 

Also, just want to point out that I live in the Naryn oblast, which has a total of 6 people per square kilometer. Let that sink in for a while.

Regardless, let's just proceed for now and see what happens. I'll convert that table above into a JSON file called oblast_info.JSON and join the information in oblast_info.JSON into our main kg.ndJSON file.[^4]

So right now, this doesn't look that good. I'll do a part 2 to hopefully improve the look.




<br>

[^1]: ```` npm install -g shapefile```` <br> ```shp2json KGZ_adm1.shp -o kg.json ```
[^2]: [https://en.wikipedia.org/wiki/Regions_of_Kyrgyzstan](https://en.wikipedia.org/wiki/Regions_of_Kyrgyzstan)
[^3]: Also, with just 9 regions we're gonna have a pretty boring looking map even without the giant density disparity. Maps always look way cooler when they have lots of different areas, giving it high resolution.
[^4]: 
