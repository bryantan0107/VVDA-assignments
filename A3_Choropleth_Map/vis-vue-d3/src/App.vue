<template>
  <div id="app">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-12">
          <YearSlider />
        </div>
      </div>
      <div class="row">
        <div class="col-md-5">
          <Scatterplot @updateStateColors="updateStateColors" />
        </div>
        <div class="col-md-7">
          <ChoroplethMap :stateColors="stateColors" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Scatterplot from "./components/Scatterplot.vue";
import ChoroplethMap from "./components/ChoroplethMap.vue";
import YearSlider from "./components/YearSlider.vue";

export default {
  name: "App",
  components: {
    Scatterplot,
    ChoroplethMap,
    YearSlider,
  },
  data() {
    return {
      stateColors: {},
    };
  },
  mounted() {
    this.$store.dispatch("loadData");
  },
  methods: {
    updateStateColors(updatedStateColors) {
      this.stateColors = updatedStateColors;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
