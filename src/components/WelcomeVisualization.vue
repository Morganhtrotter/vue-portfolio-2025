<template>
    <div class="svg-wrapper">
        <div id="canvas">
            <svg ref="svgRef" width="100%" height="100vh"></svg>
        </div>
        <div class="welcome-text-wrapper">
            <h1>Hello, my name is <span>Morgan.</span></h1>
            <h2>I'm a Front End Web Developer</h2>
            <a>View my work</a>
        </div>
    </div>
</template>
<script setup>
import * as d3 from 'd3';
import { onMounted, ref } from 'vue';

const svgRef = ref(null);
const data = [20, 50, 30, 70, 40];


//const svg = d3.select('#canvas').append('svg').attr("preserveAspectRatio", "xMinYMin meet").attr("viewBox", "0 0 960 500");

onMounted(() => {
    const svg = d3.select(svgRef.value);
      const width = 500;
      const height = 300;

      const barWidth = width / data.length;

      svg.selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('x', (d, i) => i * barWidth)
        .attr('y', d => height - d)
        .attr('width', barWidth - 2)
        .attr('height', d => d)
        .attr('fill', 'steelblue');
});
</script>
<style scoped>
svg {
    background-color: #253238;
}

.welcome-text-wrapper {
    position: relative;
    font-family: "Bungee", sans-serif;
    text-align: center;
    top: -70vh;
    font-size: 44px;
}

.welcome-text-wrapper span {
    color: red;
}
</style>