<script>
  //   export let name;
 import Demo from './components/Demo.svelte'
 import Demo2 from './components/Demo2.svelte'

</script>

<main>
	<Demo2 />
	<Demo />

</main>

<style>
  main {
    text-align: center;
    /* padding: 1em;
    max-width: 240px; */
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
