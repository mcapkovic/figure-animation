<script>
  import YouTube from "./YouTube.svelte";
  import { debounce } from "../../utils";

  const VISIBLE = "visible";
  const HIDDEN = "hidden";

  export let toggleDemo;

  let player1;
  let videoVisibility = HIDDEN;
  let videoState = -1;
  let isPlayerReady = false;
  let isFirstPlay = true;

  function playVideo() {
    player1.playVideo();

    if (isFirstPlay) {
      setTimeout(function () {
        if (videoState === 2) player1.playVideo();
        toggleDemo(false);
      }, 5000);
      isFirstPlay = false;
    } else {
      toggleDemo(false);
    }
  }

  //   function stopVideo() {
  //     // player1.seekTo(4, false);
  //     player1.stopVideo();
  //   }

  function pauseVideo() {
    player1.pauseVideo();
    toggleDemo(true);
  }

  const debouncedStateChange = debounce(function (e) {
    videoState = e.detail;
  }, 250);

  function onPlayerReady() {
    isPlayerReady = true;
    player1.playVideo();
    setTimeout(function () {
      player1.pauseVideo();
    }, 250);
  }
</script>

<div class="player-wrapper">
  {#if isPlayerReady}
    <div class="player-wrapper__buttons">
      {#if videoState === -1 || videoState === 2}
        <button on:click={playVideo}>PLAY</button>
      {:else if videoState === 1}
        <button on:click={pauseVideo}>PAUSE</button>
      {:else if videoState === 3}
        <span>BUFFERING</span>
      {/if}
      <button
        on:click={() =>
          (videoVisibility = videoVisibility === VISIBLE ? HIDDEN : VISIBLE)}
        >TOGGLE PLAYER</button
      >
      {videoState}
    </div>
  {/if}

  <div
    class={`player-wrapper__video player-wrapper__video--${videoVisibility}`}
  >
    <!-- on:StateChange={(e) => (videoState = e.detail)}   -->
    <YouTube
      videoId="EDRmJSwrVns"
      bind:this={player1}
      on:StateChange={debouncedStateChange}
      on:PlayerReady={onPlayerReady}
    />
  </div>
</div>

<style>
  .player-wrapper {
    position: absolute;
    top: 1rem;
    /* right: 0; */
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
  }
  .player-wrapper__video--visible {
    display: block;
  }
  .player-wrapper__video--hidden {
    display: none;
  }

  .player-wrapper__buttons {
    color: white;
  }
  .player-wrapper__buttons button {
    background: none;
    color: white;
    border: 1px solid white;
  }

  .player-wrapper__buttons button:active {
    background-color: #ffffff17;
  }
</style>
