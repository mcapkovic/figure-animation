<script>
  //   export let name;
  import Demo from "./components/example1/Demo.svelte";
  import Demo2 from "./components/example1/Demo2.svelte";
  import Demo3 from "./components/example1/Demo3.svelte";
  import Demo4 from "./components/example2/Game.svelte";

  let activeDemo = 4;
  let isExpanded = false;
</script>

<main>
  {#if activeDemo === 1}
    <Demo />
  {:else if activeDemo === 2}
    <Demo2 />
  {:else if activeDemo === 3}
    <Demo3 />
  {:else if activeDemo === 4}
    <Demo4 />
  {/if}

  <div class="controls">
    <button class="menu" on:click={() => (isExpanded = !isExpanded)}>☰</button>
    {#if isExpanded}
      <button on:click={() => (activeDemo = 1)}>demo</button>
      <button on:click={() => (activeDemo = 2)}>demo2</button>
      <button on:click={() => (activeDemo = 3)}>demo3</button>
      <button on:click={() => (activeDemo = 4)}>demo4</button>
    {/if}
  </div>
</main>

<style>
  main {
    text-align: center;
    /* padding: 1em;
    max-width: 240px; */
    margin: 0 auto;
  }

  .menu {
    background: none;
    color: #ffffff7d;
    border: 1px solid #ffffff7d;
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
