<!-- play on button click is not working on mobile devices -->

<script>
  function playVideo() {
    const video = document.querySelector("#video-0");
    video.src = "https://www.youtube.com/embed/EDRmJSwrVns?start=3&autoplay=1";
  }
  function stopVideo() {
    const video = document.querySelector("#video-0");
    video.src = "";
  }
</script>

<div>
  <iframe
    id="video-0"
    width="320"
    height="240"
    src="https://www.youtube.com/embed/EDRmJSwrVns?start=3"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen
  />
  <div>
    <button on:click={playVideo}> play music </button>
    <button on:click={stopVideo}> stop music </button>
  </div>
</div>

<style>
</style>
