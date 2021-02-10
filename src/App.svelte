<script>
  //   export let name;
  import Demo from "./components/example1/Demo.svelte";
  import Demo2 from "./components/example1/Demo2.svelte";
  import Demo3 from "./components/example1/Demo3.svelte";

  let activeDemo = 2;
</script>

<main>
  <!-- <Demo3 />
  <Demo2 /> -->

  {#if activeDemo=== 2}
  <Demo2 />

  {:else}
  <Demo3 />
  {/if}

  <div class="controls">
    <button on:click={() => (activeDemo = 2)}>demo2</button>
    <button on:click={() => (activeDemo = 3)}>demo3</button>
  </div>

  <Demo />
</main>

<style>
  main {
    text-align: center;
    /* padding: 1em;
    max-width: 240px; */
    margin: 0 auto;
  }

  .controls {
    position: absolute;
    top: 1rem;
    left: 1rem;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
