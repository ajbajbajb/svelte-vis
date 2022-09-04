<script lang="ts">
  import { scaleLinear, scaleOrdinal } from "d3-scale";
  import { extent } from "d3-array";
  import type { IrisEntry } from "./types";

  export let data: IrisEntry[];
  export let xDimension: string;
  export let yDimension: string;

  const height = 600;
  const width = 600;
  const buffer = 10;
  const axisSpace = 30;
  const colors = ["red", "green", "blue"];

  $: xExtent = extent(data, (d) => d[xDimension]);
  $: yExtent = extent(data, (d) => d[yDimension]);
  // console.log(xExtent);

  // javascript set that is passed a list of each flower.ƒ
  let species = Array.from(new Set(data.map((d) => d.species)));
  console.log(species);

  let colorScale = scaleOrdinal().domain(species).range(colors);
  // construct the scale by interpolating between the two values identified in the extent
  // and then restrict them to the range of the available width or height.
  $: xScale = scaleLinear()
    .domain(xExtent)
    .range([buffer + axisSpace, width - buffer - axisSpace]);
  $: yScale = scaleLinear()
    .domain(yExtent)
    .range([height - buffer - axisSpace, buffer + axisSpace]);
</script>

<svg {height} {width} transition="transform 0.4s">
  {#each data as item}
    <circle
      r="3"
      transform={`translate(${xScale(item[xDimension])} ${yScale(
        item[yDimension]
      )})`}
      fill={colorScale(item.species)}
    />
  {/each}
  {#each xScale.ticks(5) as tick}
    <g transform={`translate(${xScale(tick)} ${height - 20})`}>
      <line y1="-5" y2="0" stroke="black" />
      <text y="20" text-anchor="middle">{tick}</text>
    </g>
  {/each}
  {#each yScale.ticks(5) as tick}
    <g transform={`translate(0 ${yScale(tick)} )`}>
      <line x1="35" x2="40" stroke="black" />
      <text x="30" dominant-baseline="middle" text-anchor="end">{tick}</text>
    </g>
  {/each}
  <g transform={`translate(${width - 100}, ${height - 100})`}>
    {#each species as species, i}
      <g transform={`translate(0, ${i * 20})`}>
        <rect height="10" width="10" fill={colorScale(species)} />
        <text dominant-baseline="middle" y="5" x="20">{species}</text>
      </g>
    {/each}
  </g>
</svg>

<!-- use css animations to animate transitions -->
<style>
  svg {
    transition: transform 0.4s;
  }
</style>
