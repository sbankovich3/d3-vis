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
- <u>D</u>ata <u>D</u>riven <u>D</u>ocuments
- Functional Javascript
- Full capibility of modern browser
- Attach data to DOM elements
- Customize with CSS, HTML, and SVG
- Without tied to proprietary software
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

```javascript zoom-18
var svg = d3.select("body")
            .append("svg")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
            .attr("class", "svg")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
```

@snap[south span-100 text-gray text-08]
@[1](select body of page)
@[2](append svg)
@[3-7, zoom-12](add attributes, class, and margins)
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
@snapend

---

@snap[north-east span-100 text-white text-06]
loading & cleaning data
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
@snapend



