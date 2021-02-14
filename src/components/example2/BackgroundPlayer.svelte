<script>
  import YouTube from "./YouTube.svelte";

  const VISIBLE = "visible";
  const HIDDEN = "hidden";
  let player1;
  let videoVisibility = HIDDEN;
  function playVideo() {
    // player1.seekTo(4);
    player1.playVideo();
  }
  function stopVideo() {
    // player1.seekTo(4, false);
    player1.stopVideo();
  }
</script>

<div class="player-wrapper">
  <div>
    <button on:click={playVideo}>play</button>
    <button on:click={() => player1.pauseVideo()}>pause</button>
    <button
      on:click={() =>
        (videoVisibility = videoVisibility === VISIBLE ? HIDDEN : VISIBLE)}
      >toggle video</button
    >
  </div>
  <div
    class={`player-wrapper__video player-wrapper__video--${videoVisibility}`}
  >
    <YouTube videoId="EDRmJSwrVns" bind:this={player1} />
  </div>
</div>

<style>
  .player-wrapper {
    position: absolute;
    top: 10px;
    /* right: 0; */
    left: 50%;
    transform: translateX(-50%);
  }
  .player-wrapper__video--visible{
    /* opacity: 1; */
    display: block;
  }
  .player-wrapper__video--hidden{
    /* opacity: 0; */
    display: none;

  }
</style>
