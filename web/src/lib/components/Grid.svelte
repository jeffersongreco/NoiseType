<script lang="ts">
  import Point from "$lib/components/Point.svelte";

  type Coords = { x: number | null; y: number | null };

  let activeCoord = $state<Coords>({ x: null, y: null });

  function handlePointEnter(coords: { x: number; y: number }) {
    activeCoord = coords;
  }

  function handlePointLeave() {
    activeCoord.x = null;
    activeCoord.y = null;
  }
</script>

<div class="container">
  <div style="position: fixed; top: 0; left: 0; padding: 10px;">
    Active: {activeCoord.x}, {activeCoord.y}
  </div>

  <div class="grid">
    {#each Array(9) as _, row}
      {#each Array(9) as _, col}
        {#if row % 2 === 0 && col % 2 === 0}
          {@const pointX = row / 2 + 1}
          {@const pointY = col / 2 + 1}

          <Point
            x={pointX}
            y={pointY}
            onenter={handlePointEnter}
            onleave={handlePointLeave}
          />
        {:else}
          <div></div>
        {/if}
      {/each}
    {/each}
  </div>
</div>

<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    padding: 60px;
  }

  .grid {
    aspect-ratio: 48 / 60;
    height: stretch;
    display: grid;
    grid-template-rows: auto 1fr auto 1fr auto 1fr auto 1fr auto;
    grid-template-columns: auto 1fr auto 1fr auto 1fr auto 1fr auto;
  }
</style>
