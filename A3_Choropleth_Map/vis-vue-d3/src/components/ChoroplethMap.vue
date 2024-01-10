<template>
  <div class="vis-component" ref="map">
    <svg id="map-svg" :width="svgWidth" :height="svgHeight">
      <g class="map-group" ref="mapGroup"></g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import usMapData from "@/assets/us-states-geo.json";

export default {
  name: "ChoroplethMap",
  props: {
    stateColors: Object,
  },
  data() {
    return {
      svgWidth: 0,
      svgHeight: 400,
      svgPadding: {
        top: 0,
        right: 20,
        bottom: -100,
        left: 40,
      },
    };
  },
  mounted() {
     this.drawMap();
  },
  methods: {
    drawMap() {
      if (this.$refs.map) this.svgWidth = this.$refs.map.clientWidth;

      d3.select(this.$refs.mapGroup).attr(
        "transform",
        `translate(${this.svgPadding.left},${this.svgPadding.top})`
      );

      const projection = d3
        .geoAlbersUsa()
        .fitSize([this.svgWidth, this.svgHeight], usMapData);

      const path = d3.geoPath().projection(projection);

      d3.select(this.$refs.mapGroup)
        .selectAll("path")
        .data(usMapData.features)
        .join("path")
        .attr("d", path)
        .style("fill", (d) => this.getColorForState(d.properties.name, this.stateColors));
        console.log("stateColors: " + JSON.stringify(this.stateColors));
    },
    getColorForState(stateName, stateColors) {
      const color = stateColors[stateName];

      if (color === undefined) {
        console.warn(`Color not defined for state: ${stateName}`);
        return "black";
      }
      return color;
    },

  },
  watch: {
    stateColors: {
      handler() {
        this.drawMap();
      },
      deep: true,
    },
  },
};
</script>
