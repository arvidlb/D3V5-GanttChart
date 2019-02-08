# D3V5-GanttChart

This is a D3 v5 rendition of the [D3 Gantt Chart](https://codepen.io/jey/pen/jmClJ) D3 Gantt Chart by [Jess Peter](https://codepen.io/jey/)

I am using this Gantt Chart as a basis for a personal project and decided to make this example compatible with D3.js v5 and to make this example public since D3 v5 examples are hard to find at the moment.

## Changes

The original example is based on D3.js v3 and so a few changes had to be made to make this work with D3 v5:

### D3 Time

The main changes to the original javascript code come from the changes to the D3 time libraries:

#### Formatting time:

Changed:

`var dateFormat = d3.time.format("%Y-%m-%d");`

To:

`var dateFormat = d3.timeParse("%Y-%m-%d");`

#### Updating references:

These variables get called throughout the code so all occurrences of:

`timeScale(dateFormat.parse(x))`

Have been changed to:

`timeScale(dateFormat(x))`

A small change to referencing d3.time in axis ticks:

Changed:

`.ticks(d3.time.days, 1)`

To:

`.ticks(d3.timeDay, 1)`

And Changed:

`.tickFormat(d3.time.format('%d %b'));`

To:

`.tickFormat(d3.timeFormat('%d %b'));`


#### Setting the time scale:

Changed:

`var timeScale = d3.time.scale()
        .domain([d3.min(taskArray, function(d) {return dateFormat.parse(d.startTime);}),
                 d3.max(taskArray, function(d) {return dateFormat.parse(d.endTime);})])
        .range([0,w-150]);`
        
To:

`var timeScale = d3.scaleTime()
        .domain([d3.min(taskArray, function(d) {return dateFormat(d.startTime);}),
                 d3.max(taskArray, function(d) {return dateFormat(d.endTime);})])
        .range([0,w-150]);`

### Color Scale

A small alteration to the color scaling:

#### D3 scaleLinear():

Changed:

`var colorScale = d3.scale.linear()
    .domain([0, categories.length])
    .range(["#00B9FA", "#F95002"])
    .interpolate(d3.interpolateHcl);`
    
To:

`var colorScale = d3.scaleLinear()
    .domain([0, categories.length])
    .range(["#00B9FA", "#F95002"])
    .interpolate(d3.interpolateHcl);`
    
### D3 Axis

Referencing the D3 Axis libraries also changed from D3 v4. Instead of calling .orient('bottom') you can now directly initiate a bottom axis:

Changed:

`var xAxis = d3.svg.axis()
    .scale(timeScale)
    .orient('bottom')
    .ticks(d3.time.days, 1)
    .tickSize(-h+theTopPad+20, 0, 0)`
    
To:

`var xAxis = d3.axisBottom()
    .scale(timeScale)
    .ticks(d3.timeDay, 1)
    .tickSize(-h+theTopPad+20, 0, 0)`
