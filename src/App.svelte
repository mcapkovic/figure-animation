<script>
  //   export let name;

  let aminationType = "idle";
  let aminationDirection = "right";
  let idTimeout1 = 0;
  let idTimeout2 = 0;

  const IDLE = "idle";
  const IDLE2 = "idle2";
  const SLIDE = "slide";
  const RUN = "run";
  const CROUCH = "crouch";
  const ATTACK = "attack";
  const JUMP = "jump";

  function animationChange(type) {
    clearTimeout(idTimeout1);
    clearTimeout(idTimeout2);
    aminationType = type;
  }

  function changeDirection() {
    aminationDirection = aminationDirection === "right" ? "left" : "right";
  }

  function animationAttack(type) {
    clearTimeout(idTimeout1);
    clearTimeout(idTimeout2);
    aminationType = type;
    idTimeout2 = setTimeout(function () {
      aminationType = IDLE2;
    }, 700);
    idTimeout1 = setTimeout(function () {
      aminationType = IDLE;
    }, 3000);
  }

  function animationJump(type) {
    clearTimeout(idTimeout1);
    clearTimeout(idTimeout2);
    aminationType = type;
    idTimeout1 = setTimeout(function () {
      aminationType = IDLE;
    }, 1200);
  }

  function animationDown() {
    clearTimeout(idTimeout1);
    clearTimeout(idTimeout2);

    if (aminationType === RUN || aminationType === JUMP) {
      aminationType = SLIDE;
      idTimeout1 = setTimeout(function () {
        aminationType = RUN;
      }, 500);
    } else {
      aminationType = CROUCH;
    }
  }
</script>

<main>
  <div class={`figure figure--${aminationDirection}`}>
    <img
      class={`figure__body figure__body--${aminationType}`}
      src="/img/figure-all.png"
      alt="figure"
    />
  </div>
  <button on:click={() => animationChange(IDLE)}>{IDLE}</button>
  <!-- <button on:click={() => animationChange("idle2")}>idle2</button> -->
  <button on:click={() => animationChange(RUN)}>{RUN}</button>
  <button on:click={() => animationJump(JUMP)}>{JUMP}</button>
  <button on:click={() => animationAttack(ATTACK)}>{ATTACK}</button>
  <button on:click={() => animationDown()}>crouch/slide</button>
  <button on:click={changeDirection}>change direction</button>
  <div><a href="https://rvros.itch.io/animated-pixel-hero">hero source</a></div>
  <!-- <div >
    <img
      src="/img/background.png"
      alt="figure"
    />
  </div> -->
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

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
    border: 1px solid red;
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
