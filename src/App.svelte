<script>
  import * as d3 from "d3";
  import data from "./data/data.json";
  import avg from "./data/average.json";
  import Bar from "./lib/Bar.svelte";
  import Xaxis from "./lib/Xaxis.svelte";
  import Yaxis from "./lib/Yaxis.svelte";
  import ClipPath from "./lib/ClipPath.svelte";
  import Line from "./lib/Line.svelte";

  const scaleFactor = 1.8;

  let width = 1920 / scaleFactor;
  let height = 1080 / scaleFactor;

  const margin = {
    top: 84 / scaleFactor,
    left: (70 + 84) / scaleFactor,
    bottom: (85 + 84) / scaleFactor,
    right: 84 / scaleFactor,
  };

  const totalWidth = width + margin.left + margin.right;
  const totalHeight = height + margin.top + margin.bottom;

  const cleanedData = data.map((d) => {
    const date = d.date.toString();
    return {
      date: d.date,
      y0: +d.min,
      y1: +d.max,
    };
  });

  const minDate = new Date(cleanedData[0].date);
  const maxDate = new Date(cleanedData[cleanedData.length - 1].date);

  const xScale = d3.scaleTime().domain([minDate, maxDate]).range([0, width]);

  const yScale = d3
    .scaleLinear()
    .domain([
      d3.min(cleanedData, (d) => d.y0),
      d3.max(cleanedData, (d) => d.y1),
    ])
    .range([height, 0]);

  const colorScale = d3
    .scaleSequential(d3.interpolateSpectral)
    .domain([
      d3.max(cleanedData, (d) => d.y1),
      d3.min(cleanedData, (d) => d.y0),
    ]);
</script>

<div id="chart">
  <svg width={totalWidth} height={totalHeight}>
    <g transform="translate({margin.left},{margin.top})">
      <ClipPath
        data={avg}
        {xScale}
        {yScale}
        y0={d3.max(cleanedData, (d) => d.y1)}
        idClip={"clip-above"}
      />
      <ClipPath
        data={avg}
        {xScale}
        {yScale}
        y0={d3.min(cleanedData, (d) => d.y0)}
        idClip={"clip-below"}
      />
      <Bar
        data={cleanedData}
        category={"above"}
        width={width - margin.left}
        {xScale}
        {yScale}
        color={"#bc4b5c"}
        idClip={"clip-above"}
      />
      <Bar
        data={cleanedData}
        category={"below"}
        width={width - margin.left}
        {xScale}
        {yScale}
        color={"#3e68a1"}
        idClip={"clip-below"}
      />
      <Line data={avg} {xScale} {yScale} />
      <Xaxis {xScale} {height} />
      <Yaxis {yScale} />
    </g>
  </svg>
</div>

<style>
  #chart {
    width: 100%;
    text-align: center;
  }
</style>
