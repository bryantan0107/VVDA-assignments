<template>
  <div class="vis-component" ref="chart">
    <svg id="main-svg" :width="svgWidth" :height="svgHeight">
      <g class="chart-group" ref="chartGroup">
        <g class="axis axis-x" ref="axisX"></g>
        <g class="axis axis-y" ref="axisY"></g>
        <g class="scatter-group" ref="scatterGroup"></g>
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "ScatterPlot",
  props: {},
  data() {
    return {
      svgWidth: 0,
      svgHeight: 500,
      svgPadding: {
        top: 25,
        right: 20,
        bottom: 70,
        left: 40,
      },
      stateColors: {}, 
    };
  },
  mounted() {
    this.drawChart();
  },
  methods: {
    drawChart() {
      if (this.$refs.chart) this.svgWidth = this.$refs.chart.clientWidth;
      d3.select(this.$refs.chartGroup).attr(
        "transform",
        `translate(${this.svgPadding.left},${this.svgPadding.top})`
      );
      this.drawXAxis();
      this.drawYAxis();
      this.drawScatter();
    },
    drawXAxis() {
      d3.select(this.$refs.axisX)
        .attr(
          "transform",
          `translate(0, ${
            this.svgHeight - this.svgPadding.top - this.svgPadding.bottom
          })`
        )
        .call(d3.axisBottom(this.xScale))
        .append("text")
        .attr("y", 20)
        .attr("x", this.svgWidth / 2)
        .attr("dy", "1.5em")
        .style("text-anchor", "end")
        .attr("fill", "black")
        .text("Educational Attainment: Bachelor's Degree or Higher (%)");
    },
    drawYAxis() {
      d3.select(this.$refs.axisY)
        .call(d3.axisLeft(this.yScale))
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("y", 6)
        .attr("dy", "0.71em")
        .attr("text-anchor", "end")
        .attr("fill", "black")
        .text("Average Yearly Personal Income (in $)");
    },
    drawScatter() {
      const scatterGroup = d3.select(this.$refs.scatterGroup);

      const combinedData = this.educationalAttainmentData
        .filter(edu => this.personaleIncome.some(income => income.state === edu.state))
        .map(edu => ({
          state: edu.state,
          educationalAttainmentRate: edu.value,
          personalIncome: this.personaleIncome.find(income => income.state === edu.state).value,
        }));

        const colorMap = [
        ["#be64ac", "#8c62aa", "#3b4994"],
        ["#dfb0d6", "#a5add3", "#5698b9"],
        ["#e8e8e8", "#ace4e4", "#5ac8c8"],
      ];

      d3
        .select(this.$refs.scatterGroup)
        .selectAll(".color-rect-background")
        .data(colorMap.flatMap((row, i) => row.map((color, j) => ({ row: i, col: j, color }))))
        .join("rect")
        .attr("class", "color-rect-background")
        .attr("x", (d) => d.col * ((this.svgWidth - this.svgPadding.left - this.svgPadding.right) / 3))
        .attr("y", (d) => (2 - d.row) * ((this.svgHeight - this.svgPadding.top - this.svgPadding.bottom) / 3))
        .attr("width", (this.svgWidth - this.svgPadding.left - this.svgPadding.right) / 3)
        .attr("height", (this.svgHeight - this.svgPadding.top - this.svgPadding.bottom) / 3)
        .attr("fill", (d) => d.color);  

      scatterGroup
        .selectAll(".dot")
        .data(combinedData, d => d.state)
        .join("circle")
        .attr("class", "dot")
        .attr("cx", (d) => this.xScale(d.educationalAttainmentRate))
        .attr("cy", (d) => this.yScale(d.personalIncome))
        .attr("r", 5)
        .style("fill", (d) => {
          const row = Math.floor(this.yScale(d.personalIncome) / ((this.svgHeight - this.svgPadding.top - this.svgPadding.bottom) / 3));
          const col = Math.floor(this.xScale(d.educationalAttainmentRate) / ((this.svgWidth - this.svgPadding.left - this.svgPadding.right) / 3));

          const colorMap = [
            ["#e8e8e8", "#ace4e4", "#5ac8c8"],
            ["#dfb0d6", "#a5add3", "#5698b9"],
            ["#be64ac", "#8c62aa", "#3b4994"],
          ];

          const backgroundColor = colorMap[row][col];
          this.$set(this.stateColors, d.state, backgroundColor); // Store the color for each state
          this.$emit('updateStateColors', this.stateColors);
          return backgroundColor;
        })
        .on("mouseover", (event, d) => this.showTooltip(event, d.state))
        .on("mouseout", () => this.hideTooltip());
    },
    showTooltip(event, state) {
      const tooltip = d3.select(this.$refs.chart).append("div")
        .attr("class", "tooltip")
        .style("position", "absolute")
        .style("opacity", 0)
        .html(state);

      tooltip.transition()
        .duration(200)
        .style("opacity", 0.9)
        .style("left", (event.pageX) + "px")
        .style("top", (event.pageY - 28) + "px");
    },

    hideTooltip() {
      d3.select(".tooltip").remove();
    },
  },
  computed: {
    educationalAttainmentData: {
      get() {
        return this.$store.getters.baDegreeOrHigher;
      },
    },
    personaleIncome: {
      get() {
        return this.$store.getters.personaleIncome;
      },
    },
    dataMax() {
      return Math.max(
        d3.max(this.personaleIncome, (d) => d.value),
        85000
      );
    },
    dataMin() {
      return d3.min(this.personaleIncome, (d) => d.value);
    },
    xScale() {
      return d3
        .scaleLinear()
        .rangeRound([
          0,
          this.svgWidth - this.svgPadding.left - this.svgPadding.right,
        ])
        .domain([0, 70]);
    },
    yScale() {
      return d3
        .scaleLinear()
        .rangeRound([
          this.svgHeight - this.svgPadding.top - this.svgPadding.bottom,
          0,
        ])
        .domain([this.dataMin > 0 ? 0 : this.dataMin, this.dataMax]);
    },
  },
  watch: {
    personaleIncome: {
      handler() {
        this.drawChart();
      },
      deep: true,
    },
    educationalAttainmentData: {
      handler() {
        this.drawChart();
      },
      deep: true,
    },
  },
};
</script>

<style>
.dot {
  stroke: #fff;
}

.color-rect-background {
  fill-opacity: 0.5;
}

.tooltip {
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 10px;
  position: absolute;
  z-index: 10;
  opacity: 0;
}
</style>
