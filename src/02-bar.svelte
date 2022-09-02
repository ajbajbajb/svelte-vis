<script>
  let bridgeTraffic = [
    { name: "Henry Hudson Bridge", traffic: 58809 },
    { name: "Hugh L. Carey Tunnel", traffic: 54007 },
    { name: "Queens-Midtown Tunnel", traffic: 86081 },
    { name: "R.F.K. Memorial Bridge Manhattan Plaza", traffic: 91509 },
    { name: "George Washington Bridge", traffic: 280784 },
    { name: "Holland Tunnel", traffic: 87232 },
    { name: "Lincoln Tunnel", traffic: 109796 },
  ];
  bridgeTraffic.sort((a, b) => b.traffic - a.traffic);
</script>

<svg>
  {#each bridgeTraffic as bridge, i}
    <g transform={`translate(300 ${i * 20} )`}>
      <!-- taking index to separate the y values -->
      <rect width={bridge.traffic / 1000} height="10" fill="red" />
      <!-- the text component of svg -->
      <text
        font-size="10"
        text-anchor="end"
        y="5"
        x="-4"
        dominant-baseline="middle">{bridge.name}</text
      >
      <!-- using trace circle to show where 0 0 is for the svg -->
      <!-- <circle r="3" fill="black" /> -->
      <!-- adding text at end to show the value -->
      <text
        fill="aaa"
        y="6"
        x={bridge.traffic / 1000 + 5}
        font-size="8"
        dominant-baseline="middle"
        >{bridge.traffic.toLocaleString()}
      </text>
    </g>
  {/each}
</svg>

<style>
  svg {
    border: 1px solid #ddd;
    width: 100%;
    height: 500px;
    padding-top: 10px;
  }
</style>
