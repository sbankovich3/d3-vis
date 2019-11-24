---?color=linear-gradient(90deg, #5384AD 65%, white 35%)
@snap[north-west h4-white]
#### d3 & data visualizations
@snapend

---?color=linear-gradient(90deg, #5384AD 65%, white 35%)
@snap[north-west h4-white]
#### what is d3.js?
@snapend

@snap[west span-55]
@ul[list-spaced-bullets text-white text-09]
- **D**ata **D**riven **D**ocuments
- Functional Javascript
- Full capibility of modern browser
- Attach data to DOM elements
- Customize with CSS, HTML, and SVG
- Without tied to proprietary software
@ulend
@snapend

@snap[east span-45]
@snapend

---

@snap[north-east span-100 text-pink text-06]
loading & cleaning data
@snapend

```javascript zoom-18
d3.dsv(",", "2017ydscarr_draftdata.csv").then(function (data) {
            data.forEach(element => {
                element['athlete_id'] = +element['athlete_id'];
                element['rushing_yards'] = +element['rushing_yards'];
                element['rushing_attempts'] = +element['rushing_attempts'];
                element['Pick'] = +element['Pick'];
            })
       // callback
       })
```

@snap[south span-100 text-gray text-08]
@[1](load data and choose delimiter.)
@[2-7, zoom-13](format data with correct type)
@[8-9, zoom-12](do something with the data in a callback)
@snapend

