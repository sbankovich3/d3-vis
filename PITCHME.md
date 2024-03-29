---?color=white

@snap[north span-500 h3-black]
### d3 & data visualizations
![](d3.png)
@snapend

@snap[south span-100 text-gray text-08]
Shawnasty Bankovich
Consultant, Credera
@snapend

---?color=white
@snap[north-west h4-black]
#### what is d3.js?
@snapend

@snap[east span-250]
![](d3.png)
@snapend

@snap[west span-80]
@ul[list-spaced-bullets text-black text-09]
- <u>d</u>ata <u>d</u>riven <u>d</u>ocuments
- functional javascript
- full capibility of modern browser
- attach data to DOM elements
- customize with CSS, HTML, SVG
- without tied to proprietary software
@ulend
@snapend

---?color=white
@snap[north-west h4-black]
#### d3.js basics
@snapend

@snap[west span-80]
@ul[list-spaced-bullets text-black text-09]
- load & clean data
- select & bind data to DOM elements
- dynamically scale data & add axis
- tool tips, inputs, & animations
@ulend
@snapend

@snap[east span-250]
![](d3.png)
@snapend

---

@snap[north-east span-100 text-white text-06]
first, create svg
@snapend

```javascript zoom-14
var svg = d3.select("body")
            .append("svg")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
            .attr("class", "svg")
            .attr("transform", "translate(" 
                  + margin.left + "," + margin.top + ")");
    
```

@snap[south span-100 text-gray text-08]
@[1](select body of page)
@[2](append svg)
@[3-8, zoom-12](add attributes, class, and margins)
@[9](how do we display data on the svg?)
@snapend



---

@snap[north-east span-100 text-white text-06]
load & clean data
@snapend

```javascript zoom-18
var parseYear = d3.timeParse("%Y");
d3.dsv(",", "ncaa_nfl.csv").then(function (data) {
        data.forEach(element => {
            element['year'] = parseYear(element['year']);
            element['rushing_yards'] = +element['rushing_yards'];
            element['rushing_attempts'] = +element['rushing_attempts'];
            element['draft_pick'] = +element['draft_pick'];
        });
   // callback
});
     
```

@snap[south span-100 text-gray text-08]
@[2](load data and choose delimiter.)
@[1,3-8, zoom-13](format data with correct type)
@[9-10, zoom-12](do something with the data in a callback)
@[11, zoom-12](how do we use this data?)
@snapend

---

@snap[north-east span-100 text-white text-06]
select & bind data to DOM elements
@snapend

```javascript zoom-18
// inside callback makeGraph(data)
svg.selectAll("circle")
   .data(data)
   .enter()
   .append("circle");
      
```

@snap[south span-100 text-gray text-08]
@[2](select all circles in svg, returns empty array if none)
@[3](joins data with return from select)
@[4](joins data with placeholder for circles)
@[5](appends circle to svg)
@[6](how do we represent data with circles?)
@snapend

---

@snap[north-east span-100 text-white text-06]
dynamically scale data & add axis
@snapend

```javascript zoom-18
var xMax = d3.max(data, function (d) {
                return d[dataX]
            });
var xMin = d3.min(data, function (d) {
                return d[dataX]
            });
var xScale = d3.scaleLinear()
               .domain([xMin, xMax])
               .range([0, width - margin.right])
               .nice();
          
```

@snap[south span-100 text-gray text-08]
@[1-3](computes maximum from dataX of data)
@[4-6](computes minimum from dataX of data)
@[7-8](creates a linear scale between xMin and xMax)
@[9](sets the range of the x axis to be from the beginning of svg to end minus margin)
@[10](nice function will ensure ticks are on nice round values)
@[11](how do we use this scale for our cicles?)
@snapend

---

@snap[north-east span-100 text-white text-06]
dynamically scale data & add axis [*](https://dva-project-bucket.s3.us-east-1.amazonaws.com/visualizations/vis1-mid.html)
@snapend

```javascript zoom-18
// inside callback makeGraph(data)
svg.selectAll("circle")
    .data(data)
    .enter()
    .append("circle")
    .attr("cx", function (d, i) {
           return xScale(d[dataX]);
    })
    .attr("cy", function (d) {
           return yScale(d[dataY]);
    });
svg.append("g")
    .attr("class", "y axis")
    .call(d3.axisLeft(yScale));
     
```

@snap[south span-100 text-gray text-08]
@[6-11](uses previous scales to plot circle on svg with scaled x, y)
@[12-14](appends y axis to svg using yScale)
@[15](what else can we customize? what are these circles?)
@snapend

---

@snap[north-east span-100 text-white text-06]
tool tips
@snapend

```javascript zoom-18
// inside callback makeGraph(data)
var tip = d3.tip()
            .attr('class', 'd3-tip')
            .html(function (d) {
                  return "Name: " + d['athlete_name'];
            });
svg.call(tip);
     
```

@snap[south span-100 text-gray text-08]
@[2-3](create tip and add class)
@[4-6](return athlete name as html to display)
@[7](calls tip to svg)
@[8](what else? the undrafted players get in the way)
@snapend

---

@snap[north-east span-100 text-white text-06]
tool tips[*](https://dva-project-bucket.s3.us-east-1.amazonaws.com/visualizations/vis1-tip.html)
@snapend

```javascript zoom-18
// inside callback makeGraph(data)
svg.selectAll(".circle")
   .data(data).enter().append("circle")
   (x, y, and radius previous code)
   .style("fill", function (d) {
         return (d['Pick'] ? sequentialScale(d['Pick']) : 'black');
   })
   .style("opacity",  function (d) {
         return (d['Pick'] ? 1 : 0.25);
   })
   .on('mouseover', tip.show)
   .on('mouseout', tip.hide);
                   
```

@snap[south span-100 text-gray text-08]
@[8-10](set opacity based on if an athlete was drafted)
@[11-12](show and hide tool tip on mouseover)
@[13](what else? add select to hide undrafted players)
@snapend

---

@snap[north-east span-100 text-white text-06]
inputs [*](https://dva-project-bucket.s3.us-east-1.amazonaws.com/visualizations/vis1.html)
@snapend

```javascript zoom-18
d3.select('#selectedFilter')
  .on("change", function () {
       var data = document.getElementById("selectedFilter").value 
                == 'hide' ? data.filter(x => x.Pick > 0) : data;
       makeGraph(data)
});
     
```

@snap[south span-100 text-gray text-08]
@[1-2](on change of select, get value)
@[3-6](filter data based on value)
@snapend
---?color=black