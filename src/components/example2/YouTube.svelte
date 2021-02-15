<!-- code from https://github.com/sveltecasts/svelte-youtube -->
<script context="module">
  let YouTubeIframeAPIReady = false;
</script>

<script>
  let player;
  import { createEventDispatcher, onMount } from "svelte";
  const dispatch = createEventDispatcher();
  let divId = "player_" + parseInt(Math.random() * 100000).toString();
  export let videoId;
  export let height = "240";
  export let width = "320";

  onMount(() => {
    let ytTagUrl = "https://www.youtube.com/iframe_api";

    if (!isMyScriptLoaded(ytTagUrl)) {
      // 2. This code loads the IFrame Player API code asynchronously.
      var tag = document.createElement("script");
      tag.src = ytTagUrl;
      var firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    }

    window.onYouTubeIframeAPIReady = function () {
      //console.log('hello')
      window.dispatchEvent(new Event("YouTubeIframeAPIReady"));
    };

    window.addEventListener("YouTubeIframeAPIReady", function () {
      if (!YouTubeIframeAPIReady) YouTubeIframeAPIReady = true;
      createPlayer();
    });

    function createPlayer() {
      player = new YT.Player(divId, {
        height,
        width,
        videoId: videoId,

        events: {
          onReady: onPlayerReady,
          onStateChange: onPlayerStateChange,
        },
        playerVars: {
        //   autohide: 1,
        //   wmode: "opaque",
          origin: window.location.origin,
          start: 4,
          loop: 1,
        },
      });
    }

    if (YouTubeIframeAPIReady) {
      createPlayer(); // if the YT Script is ready, we can create our player
    }
  });

  function isMyScriptLoaded(url = "") {
    var scripts = document.getElementsByTagName("script");
    for (var i = scripts.length; i--; ) {
      if (scripts[i].src == url) return true;
    }
    return false;
  }

  function onPlayerStateChange({ data }) {
    dispatch("StateChange", data);
  }

  function onPlayerReady(data) {
    dispatch("PlayerReady", data);
  }

  export function playVideo() {
    player.playVideo();
  }

  export function stopVideo() {
    player.stopVideo();
  }

  export function pauseVideo() {
    player.pauseVideo();
  }

  export function seekTo() {
    player.seekTo();
  }
</script>

<div class="yt-component">
  <div id={divId} />
</div>
