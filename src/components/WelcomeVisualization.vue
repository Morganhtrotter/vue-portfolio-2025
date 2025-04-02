<template>
    <div class="svg-wrapper">
        <div id="canvas">
            <svg ref="svgRef" width="600px" height="600px">
                <svg id="sw-js-blob-svg" xmlns="http://www.w3.org/2000/svg" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:svgjs="http://svgjs.dev/svgjs" viewBox="0 0 600 600"><path id="mask" d="M232.0680694580078,429.5811462402344C286.38744354248047,453.7958068847656,434.5549850463867,433.50785573323566,466.88482666015625,396.59686279296875C499.2146682739258,359.68586985270184,458.90052286783856,250.65445709228516,426.047119140625,208.1151885986328C393.19371541341144,165.57592010498047,317.2774937947591,134.16230010986328,269.764404296875,141.3612518310547C222.25131479899088,148.5602035522461,147.25130462646484,203.27224985758463,140.9685821533203,251.30889892578125C134.68585968017578,299.34554799397785,177.74869537353516,405.3664855957031,232.0680694580078,429.5811462402344C286.38744354248047,453.7958068847656,434.5549850463867,433.50785573323566,466.88482666015625,396.59686279296875" fill="hsl(340, 45%, 50%)"></path></svg>
            </svg>
        </div>
        <div class="welcome-text-wrapper">
            <h1>Hello, my name is <span>Morgan.</span></h1>
            <h2>I'm a Front End Web Developer</h2>
            <a href="#my-work-header">View my work</a>
        </div>
    </div>
</template>
<script setup>
import * as d3 from 'd3';
import { onMounted, ref } from 'vue';

const svgRef = ref(null);
const mask = ref(null);

onMounted(() => {
    const svg = d3.select(svgRef.value);
    const svgMask = document.getElementById('mask');
    const svgChild = document.getElementById('sw-js-blob-svg');
    console.log(svgMask);
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

    var dots = svg.append('g')
        .selectAll('g')
        .data(finalArray)
        .enter()
        .append('g')
        .selectAll('circle')
        .data((d) => d)
        .enter()
        .append('circle')
        .attr('cx', (d) => d.dx * circleSpacing)
        .attr('cy', (d) => d.dy * circleSpacing)
        .attr('r', 0)
        .style('fill', d => d.color)
        .attr('fill', d => d.color)
        .attr('stroke', "#010101")
        .attr('stroke-width', "1px")
        .attr('stroke-opacity', "0.15")
        .attr('class', 'circle');

    dots.transition()
        .delay((d, i) => i * 20)
        .duration(2000)
        .attr('r', function(d) {
            const centerCircle = svgChild.createSVGPoint();
            centerCircle.x = d.dx * circleSpacing;
            centerCircle.y = d.dy * circleSpacing;
            if (svgMask.isPointInFill(centerCircle)) {
                return d.r
            } else {
                return 0
            }
        })

    dots.on("mouseover", function(event, d) {
            const red = d3.randomInt(1, 256)();
            const green = d3.randomInt(1, 256)();
            const blue = d3.randomInt(1, 256)();

            d3.select(this)
                .style('fill', d3.rgb(red, green, blue));
        })
        .on("mouseout", function(event, d) {
            d3.select(this)
                .transition()
                .duration(750)
                .style("fill", this.color);
        })
        .on("click", function(event, d) {
            d3.select(this)
                .style("fill", "#FFD23F")
                .attr("fill", "#FFD23F");
        })
});
</script>
<style scoped>
#canvas {
    background: rgb(2,0,36);
}

svg {
    background: linear-gradient(0deg, rgba(2,0,36,1) 6%, rgba(41,9,121,1) 63%, rgba(255,55,0,0.3701855742296919) 100%);
}

.welcome-text-wrapper {
    position: relative;
    font-family: "Bungee", sans-serif;
    text-align: center;
    top: -70vh;
    font-size: 44px;
    pointer-events: none;
    color: white;
}

.welcome-text-wrapper a {
    pointer-events: auto;
    background: linear-gradient(#2176AE 0 0) no-repeat;
    border-radius: 4px;
    background-position: 0 60px;
    transition: all 0.5s ease-out;
    padding: 8px 24px;
    color: #65717E;
}

.welcome-text-wrapper a:hover {
    cursor: pointer;
    background-position: 0;
    color: #1E1E24;
}

.welcome-text-wrapper span {
    color: #FFD23F;
}

h2 {
    color: #CBD1D8;
}

.hidden {
    display: none;
}
</style>