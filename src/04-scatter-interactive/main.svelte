<script lang="ts">
  //  importing both of these functions because we don't know the data yet.
  import type { IrisEntry } from "./types";
  // import Scatter from "./Scatter.svelte";
  import Scatter from "./Scatter.svelte";
  import DimensionSelector from "./DimensionSelector.svelte";

  async function loadData() {
    let data = await fetch("/data/iris.json");
    let json = (await data.json()) as IrisEntry[];

    return json;

    // console.log(json);
  }
  let data = loadData();
  let xDimension = "petalWidth";
  let yDimension = "petalLength";
</script>

{#await data}
  <p>Loading...</p>
{:then iris}
  <Scatter data={iris} {xDimension} {yDimension} />
  <p>
    X Dimension:
    <DimensionSelector bind:selectedColumn={xDimension} />
  </p>
  <p>
    Y Dimension:
    <DimensionSelector bind:selectedColumn={yDimension} />
  </p>
  <!-- <p>Loaded.</p> -->
{/await}
