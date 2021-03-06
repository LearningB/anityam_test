---
layout: post
title: "Taste Buds"
date: 2019-01-30
description: 
image: /assets/images/flavor.jpg
author: Bijendra
tags: 
  - SAy
---
<p>Food is the source of life. We are what we eat. Our food tells us a lot about who we are and where we come from and I feel my essence when looking on my plate. It tells me that I come from a place with majestic white mountains which are always accompanied by some rivers which have turned yellowish by  the flood while moving down the mountains. It tells me that the mountains are worthless without the colorful vegetation around it. It tells me that I love differences and mixing them. My food tells me that I love the warmth of people rather than the cold shrugs.My food defines me and I love sharing it </p>

<p>Being a Nepali, I think we have a strong food culture. We love to eat but more than that we love to eat with others. Our hospitality is best seen in our kitchens and dining tables. We are arrogantly biased on MoMo's and Bhaat. I know that those are the integral part of Nepalese identity. So today I am exploring through the food genre of Nepalese youtube scene.
</p>

<p>I took a different approach than my previous graphs. Rather than looking at the followers and ranks, I am keen to see the variety of food. The graphs are more centered on diets, meat and the dish. I do respect the six youtubers that I took the data from but for me food wins today. There are 3 biPartite graph, each showing diet, types of meat used and dish respectively moving from left to right. Each color reflects a channel as indicated by the legend on right. Data is based on the top 5 most visited video of each channel.</p>
<div id="chart"></div>
<style>
.viz-biPartite-mainBar rect{
  fill-opacity: 0;
  stroke-width: 0.5px;
  stroke: rgb(0, 0, 0);
  stroke-opacity: 0;
}
.viz-biPartite-mainBar .perc{
  fill:white;
  text-anchor:middle;
  font-size:12px;
}
.viz-biPartite-subBar{ 
	shape-rendering:crispEdges;
}
.edges{
	stroke:none;
	fill-opacity:0.3;
}
.label{
  stroke-opacity: .4;
  stroke:black;
  fill-opacity: 1;
  fill: black;
}
    </style>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="/assets/js/viz.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.13.0/d3-legend.js"></script>
<script>
width = 700;
height = 700;
var mainData=[];
var meatData=[];
var views=[];
d3.queue()
	.defer(d3.json,"/data/food.json")
	.await(start);
var color = {sajilokitchen:"#3366CC", kukmandu:"#DC3912", learntocookwithme:"#FF9900",yummyfoodworld:"#109618",yummynepalikitchen:"#990099",chefsuni:"#0099C6"};
var svg = d3.select("#chart").append("svg")
        .attr("viewBox", "0 0 900 800")
        .attr("width", 960).attr("height", 800);
svg.append("text")
    .attr("x",200).attr("y",30)
    .attr("class","header")
    .text("Veg or Non Veg");
svg.append("text").attr("x",700).attr("y",170)
    .attr("class","header")
    .text("Meat");
svg.append("text").attr("x",250).attr("y",380)
    .attr("class","header")
    .text("Dish");
var legendColor = d3.scaleOrdinal()
		.domain(["Sajilo Kitchen", "Kukmandu", "Learn To Cook With Me","Yummy Food World","Yummy Nepali Kitchen","Chef Suni" ])
        .range(["#3366CC", '#DC3912', "#FF9900","#109618","#990099","#0099C6"]);
function start(error,data){
    var legend = svg.append("g")
        .attr("class", "legendOrdinal")
        .attr("transform", "translate(700,0)");
    var legendOrdinal = d3.legendColor()
                .scale(legendColor)
                .orient("vertical")
                 .title("");
    svg.select(".legendOrdinal")
        .call(legendOrdinal);
    data.forEach(function(d){
        views.push(parseInt(d.viewCount));
		var cell = [];
		cell.push((d.name).toLowerCase());
		cell.push(d.dish);
		cell.push(+d.viewCount);
		cell.push(d.totalCount);
		cell.push(d.type.toLowerCase());
		mainData.push(cell);
		if (d.dish.toLowerCase() === "meat"){
			var newCell = [];
			newCell.push((d.name).toLowerCase());
			newCell.push(d.item);
			newCell.push(+d.viewCount);
			meatData.push(newCell);
		};
        });
	var scale = d3.scaleLog()
			.domain([d3.min(views), d3.max(views)])
			.range([1,20]);
	for (i = 0; i < meatData.length; i++) { 
			meatData[i][2] = scale(meatData[i][2])
		};
	for (i = 0; i < mainData.length; i++) { 
		mainData[i][2] =scale(mainData[i][2])
	};
	var g =[svg.append("g").attr("transform","translate(150,40)")
		,svg.append("g").attr("transform","translate(650,200)"),
		svg.append("g").attr("transform","translate(150,400)")];
	var bp=[viz.biPartite()
		.data(mainData)
		.min(10)
		.pad(1)
		.height(300)
		.width(200)
		.barSize(49)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary]),
		viz.biPartite()
		.data(meatData)
		.keyPrimary(d=>d[0])
		.keySecondary(d=>d[1])
		.value(d=>d[2])
		.min(10)
		.pad(1)
		.height(400)
		.width(200)
		.barSize(50)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary]),
		viz.biPartite()
		.data(mainData)
		.keyPrimary(d=>d[0])
		.keySecondary(d=>d[4])
		.value(d=>d[2])
		.min(10)
		.pad(1)
		.height(400)
		.width(200)
		.barSize(50)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary])
	];
[0,1,2].forEach(function(i){
	g[i].call(bp[i]);
	g[i].selectAll(".viz-biPartite-mainBar")
	.on("mouseover",mouseover)
    .on("mouseout",mouseout);
    g[i].append("text").attr("x",-50).attr("y",-8).style("text-anchor","middle").text("Channel");
    if (i === 1){
        g[i].append("text").attr("x", 200).attr("y",-8).style("text-anchor","middle").text("Meat Type");
    }else if (i === 2){
        g[i].append("text").attr("x", 250).attr("y",-8).style("text-anchor","middle").text("Dish");
    }else{
       g[i].append("text").attr("x", 250).attr("y",-8).style("text-anchor","middle").text("Diet"); 
    };
	g[i].selectAll(".viz-biPartite-mainBar").append("text").attr("class","label")
		.attr("x",d=>(d.part=="primary"? -30: 30))
		.attr("y",d=>+6)
		.text(d=>d.key)
		.attr("text-anchor",d=>(d.part=="primary"? "end": "start"));
g[i].selectAll(".viz-biPartite-mainBar").append("text").attr("class","perc")
	.attr("x",d=>(d.part=="primary"? 10: -10))
	.text(function(d){ return d3.format("0.0%")(d.percent)})
	.attr("text-anchor",d=>(d.part=="primary"? "end": "start"));
});
function mouseover(d){
	[0,1,2].forEach(function(i){
		bp[i].mouseover(d);
		g[i].selectAll(".viz-biPartite-mainBar").select(".perc")
		.text(function(d){ return d3.format("0.0%")(d.percent)});
	});
}
function mouseout(d){
	[0,1,2].forEach(function(i){
		bp[i].mouseout(d);
		g[i].selectAll(".viz-biPartite-mainBar").select(".perc")
		.text(function(d){ return d3.format("0.0%")(d.percent)});
	});
};
};
</script>