<script>
  export let  aminationType = "idle";
  export let aminationDirection = "right";
</script>

<div class={`figure figure--${aminationDirection}`}>
  <img
    class={`figure__body figure__body--${aminationType}`}
    src="/img/figure-all.png"
    alt="figure"
  />
</div>

<style>
  @keyframes jump {
    9% {
      left: 0;
      top: -370px;
    }
    18% {
      left: -250px;
      top: -370px;
    }
    28% {
      left: -500px;
      top: -370px;
    }
    37% {
      left: -750px;
      top: -370px;
    }
    46% {
      left: -1000px;
      top: -370px;
    }
    55% {
      left: -1250px;
      top: -370px;
    }
    64% {
      left: -1500px;
      top: -370px;
    }
    73% {
      left: 0;
      top: -555px;
    }
    82% {
      left: -250px;
      top: -555px;
    }
    91% {
      left: -500px;
      top: -555px;
    }
  }

  @keyframes crouch {
    0% {
      left: -1000px;
      top: 0;
    }
    25% {
      left: -1000px;
      top: 0;
    }
    50% {
      left: -1250px;
      top: 0;
    }
    75% {
      left: 0;
      top: -185px;
    }
  }

  @keyframes attack1 {
    0% {
      left: 0;
    }
    16.5% {
      left: -250px;
    }
    33% {
      left: -500px;
    }
    49.5% {
      left: -750px;
    }
    66% {
      left: -1000px;
    }
    82.5% {
      left: -1250px;
    }
  }

  @keyframes idle2 {
    0% {
      left: -750px;
    }
    25% {
      left: -1000px;
    }
    50% {
      left: -1250px;
    }
    75% {
      left: -1500px;
    }
  }

  @keyframes idle {
    0% {
      left: 0;
    }
    25% {
      left: -250px;
    }
    50% {
      left: -500px;
    }
    75% {
      left: -750px;
    }
  }

  @keyframes run {
    0% {
      left: -250px;
    }
    16.5% {
      left: -500px;
    }
    33% {
      left: -750px;
    }
    49.5% {
      left: -1000px;
    }
    66% {
      left: -1250px;
    }
    82.5% {
      left: -1500px;
    }
  }

  @keyframes slide {
    0% {
      left: -750px;
      top: -555px;
    }
    20% {
      left: -1000px;
      top: -555px;
    }
    30% {
      left: -1250px;
      top: -555px;
    }
    60% {
      left: -1500px;
      top: -555px;
    }
    80% {
      left: 0;
      top: -740px;
    }
    100% {
      left: 0;
      top: -740px;
    }
  }

  .figure {
    width: 240px;
    height: 185px;
    overflow: hidden;
    /* border: 1px solid red; */
    position: relative;
    margin: 0 auto;
  }

  .figure--left {
    transform: scaleX(-1);
  }

  .figure__body {
    position: absolute;
    top: 0;
    left: 0;
  }

  .figure__body--idle {
    top: 0;
    left: 0;
    animation-name: idle;
    animation-duration: 1s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }

  .figure__body--idle2 {
    top: -925px;
    left: -750px;
    animation-name: idle2;
    animation-duration: 1s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }

  .figure__body--run {
    top: -185px;
    left: -250px;
    animation-name: run;
    animation-duration: 0.7s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }

  .figure__body--attack {
    top: -1110px;
    left: 0;
    animation-name: attack1;
    animation-duration: 0.7s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }

  .figure__body--jump {
    top: -370px;
    left: 0;
    animation-name: jump;
    animation-duration: 1.2s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }

  .figure__body--crouch {
    top: 0;
    left: -1000px;
    animation-name: crouch;
    animation-duration: 0.7s;
    animation-iteration-count: infinite;
    animation-timing-function: step-start;
  }
  .figure__body--slide {
    left: 0;
    top: -740px;
    animation-name: slide;
    animation-duration: 0.5s;
    /* animation-iteration-count: infinite; */
    animation-timing-function: step-start;
  }
</style>
