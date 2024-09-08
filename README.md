# Data Visualization Basic with D3.js

D3.js is probably the de facto Javascript library for creating data visualizations in the web. It has a lot of modules and functions dedicated to translating raw data onto the screen, calculating specific graph layouts, rendering different chart types...it's amazingly comprehensive, but it's also a lot. 

## ✨ lessons & key takeaways 

1. [Draw a flower petal on the screen](https://observablehq.com/d/c6629eb98b76cea3)
  - SVG coordinate system
  - SVG paths and the <code>d</code> attribute
2. [Select existing petal(s) and bind movie data](https://observablehq.com/d/dd416c1d85760363)
  - <code>d3.select()</code> and <code>d3.selectAll()</code>
  - <code>selection.datum()</code> and <code>selection.data()</code>
  - <code>selection.attr()</code> and <code>selection.style()</code>
3. [Create a petal for each movie](https://observablehq.com/d/f31b2033f4fe4e9f)
  - D3's enter-append pattern
  - SVG transforms (translate)
4. [Size each petal based on its movie's rating](https://observablehq.com/d/2683357905679f61)
  - <code>d3.min()</code>, <code>d3.max()</code>, <code>d3.extent()</code>
  - D3 scales
  - SVG transforms (scale)
5. [Turn petals into flowers for each movie](https://observablehq.com/d/2683357905679f61)
  - Nesting
  - SVG transforms (rotate)
6. [Add filter by options](https://observablehq.com/d/681e1e3eb92d6f08)
  - D3's update and exit patterns
  - D3 transitions
7. [Position film flowers by the genres they share](https://observablehq.com/d/a25aafa93f553da6)
  - D3 shapes and hierarchies
  - D3 force layout

## ✨ data visualization ecosystem & resources

- [D3.js API Reference](https://github.com/d3/d3/blob/master/API.md)
- [Bl.ocks](https://bl.ocks.org/) → [Observable](http://observablehq.com/)
- [D3.js Slack](https://d3-slackin.herokuapp.com/)
- [Data Visualization Society](https://www.datavisualizationsociety.com/)

## SVG vs Canvas

SVG (Scalable Vector Graphics)
- XML syntax
- each shape is a DOM element
- **pro:** easy to get staed and interact with
- **con:** not performant at large scale

HTML5 Canvas
- Javascript API
- One Canvas element, shapes are inaccessible one drawn
- **pro:** very performant, especially for animations
- **con:** hard to interact with

*"SVG is like Illustrator and Canvas is like Photoshop"*

Deep dive about SVG: https://www.sarasoueidan.com/blog/svg-coordinate-systems/

## SVG Element Common used

![](./assets/svg_element.png)

## SVG Path

**Formula**

![](./assets/svg_path_formula.png)

Example path to draw a flower

![](./assets/flower_svg_path_breakdown.jpeg)

## D3 API

![](./assets/d3_api.jpeg)

### select() and selectAll()

- [d3.select(selector)](https://github.com/d3/d3-selection/blob/v1.4.1/README.md#select)
- [d3.selectAll(selector)](https://github.com/d3/d3-selection/blob/v1.4.1/README.md#selectAll)
- [selection.select(selector)](https://github.com/d3/d3-selection/blob/v1.4.1/README.md#selection_select)
- [selection.selectAll(selector)](https://github.com/d3/d3-selection/blob/v1.4.1/README.md#selection_selectAll)

Takes in a selector string or DOM element and returns a D3.js selection.

d3.select() and d3.selectAll() query the entire DOM. selection.select() and selection.selectAll() are restricted to the descendents of the selection.

Selections allow for method chaining.

```xml
<svg id='container'>
  <rect />
  <rect />
  <rect />
  <rect />
  <rect />
</svg>
```

```js
// wrap SVG element with d3
  const svg = d3.select('#container')
  
  // select the first path in the svg selection
  // (note: selections can be chained)
  const select = svg.select('rect')
  
  // select all the paths in the svg selection
  const selectAll = svg.selectAll('rect')  
```

### selection.data() and selection.datum()

![](./assets/data_and_datum_illustration.jpeg)

### selection.attr() and selection.style()

Selecting one or all of the SVG paths and setting attributes on them. Within a DOM element using the .attr() and .style() methods that set either the attribute or the CSS styles on elements.

```js
  const rectWidth = 50
  
  const svg = html`
    <svg width=${rectWidth * barData.length} height=100 style='border: 1px dashed'>
      <rect />
      <rect />
      <rect />
      <rect />
      <rect />
    </rect>
  `
 
  d3.select(svg).selectAll('rect')
    .data(barData)
    // calculate x-position based on its index
    .attr('x', (d, i) => i * rectWidth)
    // set height based on the bound datum
    .attr('height', d => d)
    // rest of attributes are constant values
    .attr('width', rectWidth)
    .attr('stroke-width', 3)
    .attr('stroke-dasharray', '5 5')
    .attr('stroke', 'plum')
    .attr('fill', 'pink')
  
  return svg
```

### selection.enter()

The d3.selection.enter() function in D3.js is used to create the missing elements and return the enter selection.

```js
selection.enter(); 
```

![](./assets/enter_illustration.jpeg)

## D3 Scale

D3.js scales to translate raw data into visual channels.

![](./assets/common_data_type.png)

### D3 Scale methods

This method is often used

**continuous → continuous**

- [scaleLinear()](https://github.com/d3/d3-scale#linear-scales)
- [scaleLog()](https://github.com/d3/d3-scale/tree/v2.2.2#log-scales)
- [scaleSqrt()](https://github.com/d3/d3-scale/tree/v2.2.2#scaleSqrt)
- [scaleTime()](https://github.com/d3/d3-scale/tree/v2.2.2#time-scales)

**continuous → discrete**

- [scaleQuantize()](https://github.com/d3/d3-scale/tree/v2.2.2#quantize-scales)

**discrete → discrete**

- [scaleOrdinal()](https://github.com/d3/d3-scale/tree/v2.2.2#ordinal-scales)

**discrete → continuous**

- [scaleBand()](https://github.com/d3/d3-scale/tree/v2.2.2#band-scales)

D3 Scale usage:

```js
const scale = d3.scaleLinear()
  .domain([min, max]) // raw data
  .range([min, max]) // visual channel

scale(someValue) // returns translated value

// A helper functions from d3 to get min & max values from data:

d3.min(data, d => d[someAttr])
d3.max(data, d => d[someAttr])
d3.extent(data, d => d[someAttr]) // returns [min, max]
```
