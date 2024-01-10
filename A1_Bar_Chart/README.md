# Bryan Yi Jue Tan (11930138)

## NBA Teams 3P% Visualization

This project is a data visualization of NBA teams' 3-point shooting percentages for the 2020 season. It displays the 3P% of each team in a bar chart, sorted from highest to lowest.

## Implementation

The project was implemented using HTML and JavaScript (D3.js library). Here's a brief overview of the implementation according to the Grading:

## Grade 4 (50% - 62%)

### 15%: Submitting according to the procedure

- I summitted my A1 both on Moodle and on the Uni Wien Almighty servers.
- I use FileZilla to connect to the Uni Wien Almighty servers. I summitted my index.html inside the A1 folder according to the Submission instructions.
- On Moodle I first zip all of my folder with all needed files and uploaded them in the Moodle.

### 25%: Properly setting up an html page with d3 (v6), js and css included

- I used the following code in head to set up the html page with the help of the tutoral.
  <script src="d3.min.js"></script>
  <link rel="stylesheet" type="text/css" href="styles.css">

### 22%: Properly laying out the svg element in html (aka skeleton html file)

- I used the following code in script to create the SVG canvas with the help of the tutoral. The size of the canvas is set in the styles.css.
  var canvas = d3.select("body").append("svg");

## Grade 3 (62.5% - 74.5%)

### 5%: Loading data into d3 from csv

- I used the following code to load date into d3 with the help of the tutoral. The data file was downloaded from the A1 assignment page.
  d3.csv("NBA_teams_3pp_2020.csv")

### 7.5%: Creating svg shapes (bars) based on data

- I used the following code to create the bars based on data with the help of the tutoral. The color was set in the styles.css.
  diagram.selectAll("p")
  .data(data)
  .enter()
  .append("rect")
  .attr("class", "rect-bar")
  .attr("x", d => scaleX(d.team))
  .attr("y", d => scaleY(d.percentage)+50)
  .attr("width", scaleX.bandwidth())
  .attr("height", d => 500 - scaleY(d.percentage));

## Grade 2 (75% - 87%)

### 7.5%: Showing x and y axes for bar chart

- I used the following code to create x and y axes for bar chart with the help of the tutoral. The font size was set in the styles.css.
  let axisX = d3.axisBottom();
  axisX.scale(scaleX);

diagram.append("g")
.attr("class", "axis-tick")
.call(axisX)
.attr("transform", "translate(0,550)"); //+50

let axisY = d3.axisLeft();
axisY.scale(scaleY);

diagram.append("g")
.attr("class", "axis-tick")
.call(axisY)
.attr("transform", "translate(0, 50)");

### 5%: Add labels to x and y axes (as children elements to the SVG using d3)

- I used the following code to add labels to x and y axes with the help of the tutoral. The font size and weight was set in the styles.css.
  diagram.append("text")
  .attr("class", "axis-label")
  .attr("x", 750)
  .attr("y", 590)
  .text("Teams");

diagram.append("text")
.attr("class", "axis-label")
.attr("x", -55)
.attr("y", 250)
.text("3P%");

## Grade 1 (87.5% - 100%)

### 7%: Label the data points and the scale on x and y axes (as children elements to the SVG using d3)

### 6%: Display the bars sorted by the 3P% from left (highest) to right (lowest)

It was done by the following code with the help of the internat.
data.sort((a, b) => b.percentage - a.percentage);

## Data Source

- https://www.w3schools.com/html/html_css.asp
- https://www.d3indepth.com/scales/
- https://observablehq.com/@d3/d3-scaleband
- https://d3js.org/d3-axis
- https://stackoverflow.com/questions/11189284/d3-axis-labeling
- https://www.geeksforgeeks.org/sort-an-array-in-reverse-order-javascript/#:~:text=Approach%202%3A-,Initialize%20an%20array%20with%20some%20values.,will%20sort%20in%20descending%20order.
