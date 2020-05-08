<template>
  <svg width="500" height="270">
    <g style="transform: translate(0, 10px)">
      <path :d="line" />
    </g>
    <g ref="axisX"></g>
    <g ref="axisY"></g>
  </svg>
</template>

<script>
  import * as d3 from 'd3';
  import myJson from '../../public/zadanie-wykres'

  export default {
    name: 'vue-line-chart',
    data() {
      return {
        data: myJson,
        line: '',
      };
    },
    mounted() {
      this.calculatePath();
    },
    methods: {
      getScales() {
        const x = d3.scaleLinear()
          .range([0, 430])
          .domain(d3.extent(this.data,(d => d["case A"])));
        const y = d3.scaleLinear()
          .range([210, 0])
          .domain(d3.extent(this.data,(d => d["case B"])));

        d3.select(this.$refs.axisX).call(d3.axisBottom(x));
        d3.select(this.$refs.axisY).call(d3.axisLeft(y));

        return { x, y };
      },
      calculatePath() {
        const scale = this.getScales();
        const path = d3.line()
          .x(d => scale.x(d["case A"]))
          .y(d => scale.y(d["case B"]));
        this.line = path(this.data);
      },
    },
  };
</script>

<style scoped>
  svg
    {margin: 25px;}
  path
    {fill: none;
    stroke: #76BF8A;
    stroke-width: 3px;}
</style>