<script>
  import Hero from "./Hero.svelte";
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
    const prevAnimation = aminationType;
    aminationType = type;
    idTimeout1 = setTimeout(function () {
      aminationType = prevAnimation;
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

  let background9X = 0;
  let background8X = 0;
  let background5X = 0;
  let background4X = 0;
  let background3X = 0;
  let backgroundX = 0;
  let moveBackroungID;

  function moveX(timestamp) {
    if (aminationDirection === "right") {
      background9X -= 5;
      background8X -= 4;
      background5X -= 3;
      background4X -= 2;
      background3X -= 1;
      backgroundX -= 5;
    } else {
      background9X += 5;
      background8X += 4;
      background5X += 3;
      background4X += 2;
      background3X += 1;
      backgroundX += 5;
    }
    moveBackroungID = requestAnimationFrame(moveX);
  }

  function start() {
    moveBackroungID = requestAnimationFrame(moveX);
    animationChange(RUN);
  }
  function stop() {
    cancelAnimationFrame(moveBackroungID);
    moveBackroungID = null;
    animationChange(IDLE);
  }

  function handleKeydown(event) {
    // key = event.key;
    // keyCode = event.keyCode;

    if (event.keyCode === 39) {
      aminationDirection = "right";
      animationChange(RUN);
      if (!moveBackroungID) moveBackroungID = requestAnimationFrame(moveX);
      // moveX()
    } else if (event.keyCode === 37) {
      // cancelAnimationFrame(moveBackroungID);
      // moveBackroungID=null;
      if (!moveBackroungID) moveBackroungID = requestAnimationFrame(moveX);

      aminationDirection = "left";
      animationChange(RUN);
    }
    // else if(event.keyCode === 38){
    //     animationJump(JUMP)
    // }else if(event.keyCode === 40){
    //     animationDown()
    // }else if(event.keyCode === 32){
    //     animationAttack(ATTACK)
    // }
  }

  function handleKeyup(event) {
    // console.log(event)
    if (
      event.keyCode === 39 &&
      aminationDirection === "right" &&
      aminationType === RUN
    ) {
      animationChange(IDLE);
      cancelAnimationFrame(moveBackroungID);
      moveBackroungID = null;
    } else if (
      event.keyCode === 37 &&
      aminationDirection === "left" &&
      aminationType === RUN
    ) {
      animationChange(IDLE);
      cancelAnimationFrame(moveBackroungID);
      moveBackroungID = null;
    }
    // else if(event.keyCode === 40){
    //     animationChange(IDLE)
    // }
  }
</script>

<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} />

<div class="demo">
  <div class="demo-game">
 

    <!-- <div
    class="test"
    style={`background-position: 
   ${background9X}px 0,
   ${background8X}px 0, 
   ${background8X}px 0, 
   ${background8X}px 0, 
   ${background5X}px 0,
   0 0,
   0 0;`}
  /> -->

    <div
      class="test2 demo2-game__background"
      style={`background-position: 
     ${background9X}px 0,
     ${background8X}px 0, 
     ${background8X}px 0, 
     ${background8X}px 0, 
     ${background5X}px 0, 
     ${background5X}px 0,
     ${background4X}px 0,
     ${background4X}px 0,
     ${background3X}px 0,
     0 0,
     0 0;`}
    />

    <!-- <div class="test3">
    <div class="test3-10" style={`background-position: 0 0;`} />
    <div class="test3-9" style={`background-position: 0 0;`} />
    <div class="test3-8" style={`background-position: ${background3X}px 0;`} />
    <div class="test3-7" style={`background-position: ${background4X}px 0;`} />
    <div class="test3-6" style={`background-position: ${background4X}px 0;`} />
    <div class="test3-5" style={`background-position: ${background5X}px 0;`} />
    <div class="test3-4" style={`background-position: ${background5X}px 0;`} />
    <div class="test3-3" style={`background-position: ${background8X}px 0;`} />
    <div class="test3-2" style={`background-position: ${background8X}px 0;`} />
    <div class="test3-1" style={`background-position: ${background8X}px 0;`} />
    <div class="test3-0" style={`background-position: ${background9X}px 0;`} />
  </div> -->

    <!-- <div
  class="test"
  style={`background-position: ${background9X}px 0;`}
/> -->
<div class="demo2-game__controls">
  <div>use left/right arrows or button</div>
  {#if aminationType !== RUN}
    <button on:click={start}>run</button>
  {:else}
    <button on:click={stop}>stop</button>
  {/if}
</div>


    <div class="hero-wrapper">
      <Hero {aminationType} {aminationDirection} />
    </div>
  </div>
  
  <!-- <button on:click={start}>start</button>
  <button on:click={stop}>stop</button> -->

  <!-- <button on:click={() => animationChange(IDLE)}>{IDLE}</button> -->
  <!-- <button on:click={() => animationChange("idle2")}>idle2</button> -->
  <!-- <button on:click={() => animationChange(RUN)}>{RUN}</button>
    <button on:click={() => animationJump(JUMP)}>{JUMP}</button>
    <button on:click={() => animationAttack(ATTACK)}>{ATTACK}</button>
    <button on:click={() => animationDown()}>crouch/slide</button>
    <button on:click={changeDirection}>change direction</button> -->
  <!-- <div><a href="https://rvros.itch.io/animated-pixel-hero">hero source</a></div> -->
  <!-- <div >
        <img
          src="/img/background.png"
          alt="figure"
        />
      </div> -->
</div>

<style>
  .test {
    /* background-image: url('/img/background.png'); */
    /* background-image: url("/img/favicon.png"); */
    background-image: url("/img/background_layers/Layer_0000_9.png"),
      url("/img/background_layers/Layer_0001_8.png"),
      url("/img/background_layers/Layer_0002_7.png"),
      url("/img/background_layers/Layer_0003_6.png"),
      /* url("/img/background_layers/Layer_0004_Lights.png"), */
        url("/img/background_layers/Layer_0005_5.png"),
      /* url("/img/background_layers/Layer_0006_4.png"), */
        /* url("/img/background_layers/Layer_0007_Lights.png"), */
        /* url("/img/background_layers/Layer_0008_3.png"), */
        url("/img/background_layers/Layer_0009_2.png"),
      url("/img/background_layers/Layer_0010_1.png");
    height: 793px;
    width: 100%;
    /* background-position: 20px -90px; */
    will-change: background-position;
  }
  .test2 {
    background-image: url("/img/background_layers/Layer_0000_9.png"),
      url("/img/background_layers/Layer_0001_8.png"),
      url("/img/background_layers/Layer_0002_7.png"),
      url("/img/background_layers/Layer_0003_6.png"),
      url("/img/background_layers/Layer_0004_Lights.png"),
      url("/img/background_layers/Layer_0005_5.png"),
      url("/img/background_layers/Layer_0006_4.png"),
      url("/img/background_layers/Layer_0007_Lights.png"),
      url("/img/background_layers/Layer_0008_3.png"),
      url("/img/background_layers/Layer_0009_2.png"),
      url("/img/background_layers/Layer_0010_1.png");
    height: 793px;
    width: 100%;
    /* background-position: 20px -90px; */
    will-change: background-position;
  }

  .test3 {
    position: relative;
  }

  .test3-0 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0000_9.png");
  }

  .test3-1 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0001_8.png");
  }
  .test3-2 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0002_7.png");
  }

  .test3-3 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0003_6.png");
  }
  .test3-4 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0004_Lights.png");
  }
  .test3-5 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0005_5.png");
  }
  .test3-6 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0006_4.png");
  }
  .test3-7 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0007_Lights.png");
  }
  .test3-8 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0008_3.png");
  }
  .test3-9 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0009_2.png");
  }
  .test3-10 {
    position: absolute;
    top: 0;
    left: 0;
    height: 793px;
    width: 100%;
    background-image: url("/img/background_layers/Layer_0010_1.png");
  }

  /* url("/img/background_layers/Layer_0002_7.png"),
    url("/img/background_layers/Layer_0003_6.png"),
    url("/img/background_layers/Layer_0004_Lights.png"),
    ,
    ,
    ,
   ,
    ,
     */
  .demo {
    text-align: center;
    /* padding: 1em; */
    margin: 0 auto;
  }

  .demo-game {
    position: relative;
    height: 100vh;
    background-color: #7693b3;
  }

  .demo2-game__controls {
    position: absolute;
    top: 2rem;
    left: 50%;
    transform: translateX(-50%);
  }

  .demo2-game__background{
    position: absolute;
    left: 0;
    bottom:0;
  }

  .hero-wrapper {
    position: absolute;
    bottom: 22px;
    left: 150px;
    transform: scale(0.6);
  }
</style>
