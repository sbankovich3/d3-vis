---?color=linear-gradient(90deg, #9c1616 85%, white 15%)
@snap[north-west h4-white]
#### d3 & data visualizations
@snapend

---?color=linear-gradient(90deg, #9c1616 85%, white 15%)
@snap[north-west h4-white]
#### what is d3.js?
@snapend

@snap[west span-80]
@ul[list-spaced-bullets text-white text-09]
- <u>d</u>ata <u>d</u>riven <u>d</u>ocuments
- functional javascript
- full capibility of modern browser
- attach data to DOM elements
- customize with CSS, HTML, and SVG
- without tied to proprietary software
@ulend
@snapend

@snap[east span-45]
@snapend

---?color=linear-gradient(90deg, #9c1616 85%, white 15%)
@snap[north-west h4-white]
#### d3.js basics
@snapend

@snap[west span-80]
@ul[list-spaced-bullets text-white text-09]
- loading & cleaning data
- selecting & binding data to DOM elements
- dynamically scaling data & add axis
- tool tips, inputs, & animations
@ulend
@snapend

@snap[east span-45]
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
loading & cleaning data
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
selecting & binding data to DOM elements
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
@[6](how do we put circles where we want them?)
@snapend

---

@snap[north-east span-100 text-white text-06]
dynamically scaling data & add axis
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
@[4-6](computes maximum from dataX of data)
@[7-8](creates a linear scale between xMin and xMax)
@[9](sets the range of the x axis to be from the beginning of svg to end minus margin)
@[10](nice function will ensure ticks are on nice round values)
@[11](how do we use this scale for our cicles?)
@snapend

---

@snap[north-east span-100 text-white text-06]
dynamically scaling data & add axis [*](https://www.example.com)
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
@[15](appends y axis to svg using yScale)
@[15]what else can we customize?
@snapend