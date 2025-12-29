<script lang="ts">
  interface Props {
    x: number;
    y: number;
    onenter?: (coords: { x: number; y: number }) => void;
    onleave?: () => void;
  }

  let { x, y, onenter, onleave }: Props = $props();

  function handleMouseEnter() {
    onenter?.({ x, y });
  }

  function handleMouseLeave() {
    onleave?.();
  }

  function handlePointClick() {
    console.log(`Point clicked.`);
  }
</script>

<button
  class="point"
  onmouseenter={handleMouseEnter}
  onmouseleave={handleMouseLeave}
  onclick={() => handlePointClick()}
  aria-label={`Point ${x}, ${y}`}
>
</button>

<style>
  .point {
    position: relative;
    aspect-ratio: 1 / 1;
    width: 16px;
    background-color: white;
  }

  .point:hover {
    background-color: tomato;
  }

  .point::after {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    width: calc(16px * 5);
    height: calc(16px * 5);

    background-color: transparent;
  }
</style>
