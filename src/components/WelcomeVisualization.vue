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
    const circleSpacing = 20;

    let horizontalCircles = [...Array(200)];
    let verticalCircles = [...Array(100)];
    let finalArray = [];
    verticalCircles.forEach((item, i) => {
        const nodes = horizontalCircles.map((temp, j) => {
            const dx = j;
            const dy = i;
            const r = d3.randomUniform(4, 8)();
            const colorIndex = d3.randomInt(1, 8)();
            const red = d3.randomInt(40, 100)();
            const green = d3.randomInt(10, 230)();
            const blue = d3.randomInt(60, 250)();
            const alpha = Math.random();
            return {
                r: r,
                dx: dx,
                dy: dy,
                color: d3.rgb(red, green, blue, 0.15),
            };
        });
        finalArray.push(nodes);
    });

    svg
        .append('g')
        .selectAll('g')
        .data(finalArray)
        .enter()
        .append('g')
        .selectAll('circle')
        .data((d) => d)
        .enter()
        .append("circle")
        .attr('cx', (d) => d.dx * circleSpacing)
        .attr('cy', (d) => d.dy * circleSpacing)
        .attr('r', d => d.r)
        .attr('fill', d => d.color)
        .attr('stroke', "#010101")
        .attr('stroke-width', "1px")
        .attr('stroke-opacity', "0.15")
        .attr('class', 'circle')
        .on("mouseover", function(event, d) {
            d3.select(this)
                .style("fill", "red")
                .style("stroke", "black")
                .style("stroke-width", "2px");
        })
        .on("mouseout", function(event, d) {
            d3.select(this)
                .transition()
                .duration(750)
                .style("fill", this.color);
        })
        .on("click", function(event, d) {
            d3.select(this)
                .style("stroke", "white");
        });
});
</script>
<style scoped>
svg {
    background-color: #272838;
}

.welcome-text-wrapper {
    position: relative;
    font-family: "Bungee", sans-serif;
    text-align: center;
    top: -70vh;
    font-size: 44px;
    pointer-events: none;
}

.welcome-text-wrapper span {
    color: red;
}
</style>