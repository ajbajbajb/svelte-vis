<script lang="ts">
  //  importing both of these functions because we don't know the data yet.
  import { scaleLinear, scaleOrdinal } from "d3-scale";
  import { extent } from "d3-array";
  const height = 600;
  const width = 600;
  const buffer = 10;
  const axisSpace = 30;
  const colors = ["red", "green", "blue"];

  interface IrisEntry {
    sepalLength: number;
    sepalWidth: number;
    petalLength: number;
    petalWidth: number;
    species: string;
  }
  async function loadData() {
    let data = await fetch("/data/iris.json");
    let json = (await data.json()) as IrisEntry[];

    // console.log(json);
    let xExtent = extent(json, (d) => d.petalLength);
    let yExtent = extent(json, (d) => d.petalWidth);
    console.log(xExtent);

    // javascript set that is passed a list of each flower.
    let species = new Set(json.map((d) => d.species));
    console.log(species);

    let colorScale = scaleOrdinal().domain(Array.from(species)).range(colors);
    // construct the scale by interpolating between the two values identified in the extent
    // and then restrict them to the range of the available width or height.
    let xScale = scaleLinear()
      .domain(xExtent)
      .range([buffer + axisSpace, width - buffer - axisSpace]);
    let yScale = scaleLinear()
      .domain(yExtent)
      .range([height - buffer - axisSpace, buffer + axisSpace]);

    return {
      data: json,
      xScale,
      yScale,
      colorScale,
      species: Array.from(species),
    };
  }
  let data = loadData();
</script>

{#await data}
  <p>Loading...</p>
{:then iris}
  <svg {height} {width}>
    {#each iris.data as item}
      <circle
        r="3"
        cx={iris.xScale(item.petalLength)}
        cy={iris.yScale(item.petalWidth)}
        fill={iris.colorScale(item.species)}
      />
    {/each}
    {#each iris.xScale.ticks(5) as tick}
      <g transform={`translate(${iris.xScale(tick)} ${height - 20})`}>
        <line y1="-5" y2="0" stroke="black" />
        <text y="20" text-anchor="middle">{tick}</text>
      </g>
    {/each}
    {#each iris.yScale.ticks(5) as tick}
      <g transform={`translate(0 ${iris.yScale(tick)} )`}>
        <line x1="35" x2="40" stroke="black" />
        <text x="30" dominant-baseline="middle" text-anchor="end">{tick}</text>
      </g>
    {/each}
    <g transform={`translate(${width - 100}, ${height - 100})`}>
      {#each iris.species as species, i}
        <g transform={`translate(0, ${i * 20})`}>
          <rect height="10" width="10" fill={iris.colorScale(species)} />
          <text dominant-baseline="middle" y="5" x="20">{species}</text>
        </g>
      {/each}
    </g>
  </svg>
  <!-- <p>Loaded.</p> -->
{/await}
