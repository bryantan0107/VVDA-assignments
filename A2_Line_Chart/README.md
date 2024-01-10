# Bryan Yi Jue Tan (11930138)

## Burglary Rates per State in the USA (1984-2014) Visualization

This project is a data visualization of Burglary Rates per State in the USA from year 1984 to 2014. It displays the Burglary Rates of each State in the USA in a line chart.

## Implementation

The project was implemented using HTML and JavaScript (D3.js library). Here's a brief overview of the implementation according to the Grading:

## Grade 4 (50% - 62%)

### 10%: Submitting according to the procedure

- I summitted my A2 both on Moodle and on the Uni Wien Almighty servers.
- I use FileZilla to connect to the Uni Wien Almighty servers. I summitted my files inside the A2 folder according to the Submission instructions.
- On Moodle I first zip all of my folder with all needed files and uploaded them in the Moodle.

### 17%: Loading the data properly into d3

- I used the following code to load date into d3 with the help of the tutoral.

  d3.csv("usa_burglary_rates_1984-2014.csv")

### 35%: Creating a line chart with each line representing one state

- I used the following code to create the lines based on data with the help of the tutoral. The color was set in the styles.css.

  diagram.selectAll("lines")
  .append("g")
  .data(groupData)
  .enter()
  .append("path")
  .attr("class", "steelblue-line")
  .attr("d", d => line(d[1]))
  .attr("fill", "none")
  .on("mouseover", lineOver)
  .on("mouseout", lineOut)
  .on("click", lineClick);

## Grade 3 (62.5% - 74.5%)

### 5%: Label x and y axes: 1. "Years" underneath the x-axis, 2. "Burglary Rate (per 100.000 people)" on the left side of the y-axis

- I used the following code to add labels to x and y axes with the help of the tutoral. The font size and weight was set in the styles.css.

  // Add label to axisX  
  diagram.append("text")
  .attr("class", "axis-label")
  .attr("x", 600)
  .attr("y", 550)
  .text("Years");

  // Add label to axisY  
  diagram.append("text")
  .attr("class", "axis-label")
  .attr("x", -40)
  .attr("y", 250)
  .text("Burglary Rate")
  .append("tspan")
  .attr("x", -55)
  .attr("dy", 16)
  .text("(per 100.000 people)");

### 7.5%: Label the scales on x and y axes: select the intervals and formatting that you find useful / informative

- I used the following code to add labels to x and y axes with the help of the tutoral.

  // Create axisX
  let axisX = d3.axisBottom()
  .scale(scaleX)
  .tickFormat(d3.format("d"));

  diagram.append("g").call(axisX)
  .attr("transform", "translate(0,500)")

  // Create axisY
  let axisY = d3.axisLeft()
  .scale(scaleY);

  diagram.append("g").call(axisY)
  .attr("transform", "translate(100,100)")

## Grade 2 (75% - 87%)

### 7.5%: On mouseover over a line, the line gets higlighted (select a highlight color that you find useful / informative) and the name of that state is shown above / next to the line

- I used the following functions lineOver and lineOut to highlight the line and show the name of the state next to the line with the help of the tutoral. After that I add the functions into the line chart (.on("mouseover", lineOver) and .on("mouseout", lineOut))

/_ INTERACTION _/
function lineOver(event, d) {
d3.select(this)
.classed("lineblue", false)
.classed("linered", true);

// Show the state name above the line
diagram.append("text")
.attr("class", "state-label")
.attr("x", 1210)
.attr("y", scaleY(d[1]d[1].length - 1].value)+100)
.text(d[0]);
}

function lineOut() {
d3.select(this)
.classed("lineblue", true)
.classed("linered", false);

// Remove the state name label
diagram.select(".state-label").remove();
}

### 5%: On mouseclick, the highlight color of the line and the text containing the state's name become permanently visible.

- I used the following code to make the highlight color and the text permantly visible. It works but there's somehow some bugs with the text that I cannot fix.

function lineClick(event, d) {
d3.select(this)
.classed("lineblue", false)
.classed("linered", true);

// Show the state name above the line
diagram.append("text")
.attr("class", "state-label")
.attr("x", 1210)
.attr("y", scaleY(d[1]d[1].length - 1].value)+100)
.text(d[0]);

d3.select(this).on("mouseout", null);
}

## Grade 1 (87.5% - 100%)

### 12%: Implement a context area underneath the line chart, which allows brushing a certain period of years that makes the line chart focus on that period.

### 1%: Add a title (an HTML "h1" element on top) summarizing the visualization

## Data Source

- https://www.geeksforgeeks.org/d3-js-axis-tickformat-function/
- https://www.w3schools.com/jsref/event_onclick.asp
- https://stackoverflow.com/questions/36812021/mouseout-and-click
