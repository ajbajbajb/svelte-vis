<script lang="ts">
  //  importing both of these functions because we don't know the data yet.
  import type { IrisEntry } from "./types";
  import Scatter from "./Scatter.svelte";

  async function loadData() {
    let data = await fetch("/data/iris.json");
    let json = (await data.json()) as IrisEntry[];

    return json;

    // console.log(json);
  }
  let data = loadData();
</script>

{#await data}
  <p>Loading...</p>
{:then iris}
  <Scatter data={iris} />
  <!-- <p>Loaded.</p> -->
{/await}
