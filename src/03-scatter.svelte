<script lang="ts">
  import Bar from "./02-bar.svelte";
  //  importing both of these functions because we don't know the data yet.
  import { scaleLinear } from "d3-scale";
  import { extent } from "d3-array";

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
    console.log(xExtent);
    return json;
  }
  let data = loadData();
</script>

{#await data}
  <p>Loading...</p>
{:then iris}
  <p>Loaded.</p>
{/await}
