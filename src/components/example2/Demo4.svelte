<script>
  import { onMount } from "svelte";
  import { getHeroAnimation } from "./animations";
  import YouTube0 from "./YouTube0.svelte";
  import BackgroundPlayer from "./BackgroundPlayer.svelte";
  import DemoButtons from "./DemoButtons.svelte";
  import {
    IDLE,
    IDLE_2,
    SLIDE,
    RUN,
    CROUCH,
    ATTACK,
    ATTACK_2,
    ATTACK_3,
    ATTACK_4,
    ATTACK_5,
    ATTACK_6,
    JUMP,
    JUMP_2,
    RIGHT,
    LEFT,
  } from "./constants";

  let isMoving = false;
  let aminationDirection = RIGHT;
  let frames = 0;
  let yOffset = 0;
  let lastPressedKey = -1;

  // SELECT CVS
  let cvs;
  let ctx;

  // LOAD IMAGE
  const heroImg = new Image();
  const layer_0000 = new Image();
  const layer_0001 = new Image();
  const layer_0002 = new Image();
  const layer_0003 = new Image();
  const layer_0004 = new Image();
  const layer_0005 = new Image();
  const layer_0006 = new Image();
  const layer_0007 = new Image();
  const layer_0008 = new Image();
  const layer_0009 = new Image();
  const layer_0010 = new Image();

  heroImg.src = "/img/adventurer-v1.5-Sheet-3x.png";
  layer_0000.src = "/img/background_layers/Layer_0000_9.png";
  layer_0001.src = "/img/background_layers/Layer_0001_8.png";
  layer_0002.src = "/img/background_layers/Layer_0002_7.png";
  layer_0003.src = "/img/background_layers/Layer_0003_6.png";
  layer_0004.src = "/img/background_layers/Layer_0004_Lights.png";
  layer_0005.src = "/img/background_layers/Layer_0005_5.png";
  layer_0006.src = "/img/background_layers/Layer_0006_4.png";
  layer_0007.src = "/img/background_layers/Layer_0007_Lights.png";
  layer_0008.src = "/img/background_layers/Layer_0008_3.png";
  layer_0009.src = "/img/background_layers/Layer_0009_2.png";
  layer_0010.src = "/img/background_layers/Layer_0010_1.png";

  // BACKGROUND
  const bg = {
    sX: 0,
    sY: 0,
    w: 928,
    h: 793,
    dx: 0,
    dy: 0,

    draw: function () {
      // console.log(ctx)
      ctx.drawImage(layer_0009, 0, yOffset);
      ctx.drawImage(
        layer_0009,
        this.sX,
        this.sY,
        this.w,
        this.h,
        this.dx + this.w,
        yOffset,
        this.w,
        this.h
      );
    },
  };

  const foregroundLayers = [
    { img: layer_0008, xName: "dx8", increment: 1 },
    { img: layer_0007, xName: "dx7", increment: 1 },
    { img: layer_0006, xName: "dx6", increment: 2 },
    { img: layer_0005, xName: "dx5", increment: 3 },
    { img: layer_0004, xName: "dx4", increment: 3 },
    { img: layer_0003, xName: "dx3", increment: 4 },
    { img: layer_0002, xName: "dx2", increment: 4 },
    { img: layer_0001, xName: "dx1", increment: 4 },
    { img: layer_0000, xName: "dx0", increment: 5 },
  ];

  // FOREGROUND
  const fg = {
    sX: 0,
    sY: 0,
    w: 928,
    h: 793,
    dx: 0,
    dy: 0,

    dx8: 0,
    dx7: 0,
    dx6: 0,
    dx5: 0,
    dx4: 0,
    dx3: 0,
    dx2: 0,
    dx1: 0,
    dx0: 0,

    draw: function () {
      foregroundLayers.forEach((layer) => {
        ctx.drawImage(layer.img, this[layer.xName] - 2 * this.w, yOffset);
        ctx.drawImage(layer.img, this[layer.xName] - this.w, yOffset);
        ctx.drawImage(layer.img, this[layer.xName], yOffset);
        ctx.drawImage(layer.img, this[layer.xName] + this.w, yOffset);
        ctx.drawImage(layer.img, this[layer.xName] + 2 * this.w, yOffset);
      });
    },

    update: function () {
      if (isMoving)
        foregroundLayers.forEach((layer) => {
          this[layer.xName] =
            aminationDirection === RIGHT
              ? (this[layer.xName] - layer.increment) % this.w
              : (this[layer.xName] + layer.increment) % this.w;
        });
    },
  };

  const hero = {
    dX: 180, // hero position
    dY: 677, // hero position
    w: 50 * 3, // hero frame width
    h: 37 * 3, // hero frame height

    frame: 0,
    period: 15,

    animationType: IDLE,
    defaultAnimationType: IDLE,

    draw: function () {
      const { w, h, dX, dY } = this;
      const { sX, sY } = this.animation[this.frame];

      if (aminationDirection === LEFT) {
        ctx.save();
        ctx.scale(-1, 1);
        ctx.drawImage(
          heroImg,
          sX,
          sY,
          w,
          h,
          -dX - w / 2,
          dY - h / 2 + yOffset,
          w,
          h
        );
        ctx.restore();
      } else {
        ctx.drawImage(
          heroImg,
          sX,
          sY,
          w,
          h,
          dX - w / 2,
          dY - h / 2 + yOffset,
          w,
          h
        ); // draw hero in the center of the location (dx,dy)
      }

      if (
        this.frame === this.animation.length - 1 &&
        this.animationType !== RUN &&
        this.animationType !== IDLE &&
        this.animationType !== IDLE_2 &&
        this.animationType !== CROUCH
      )
        this.animationType = this.defaultAnimationType;
    },

    update: function () {
      this.animation = getHeroAnimation(this.animationType, hero.w, hero.h);

      switch (this.animationType) {
        case IDLE:
          this.period = 15;
          break;
        case CROUCH:
          this.period = 15;
          break;
        case IDLE_2:
          this.period = 15;
          break;
        default:
          this.period = 8;
          break;
      }

      //   console.log(heroAminationType)
      this.frame += frames % this.period == 0 ? 1 : 0;
      this.frame = this.frame % this.animation.length;
    },
  };
  
  // DRAW
  function draw() {
    ctx.fillStyle = "#7693b3";
    ctx.fillRect(0, 0, cvs.width, cvs.height);
    ctx.fillStyle = "#0d1222";
    ctx.fillRect(0, cvs.height / 2, cvs.width, cvs.height);
    bg.draw();
    fg.draw();
    hero.draw();
  }

  // UPDATE
  function update() {
    hero.update();
    fg.update();
  }

  // LOOP
  function loop() {
    update();
    draw();
    frames++;
    requestAnimationFrame(loop);
  }

  onMount(() => {
    cvs = document.querySelector("#game-canvas");
    cvs.width = window.innerWidth - 18;
    cvs.height = window.innerHeight;
    yOffset = cvs.height > 793 ? (cvs.height - 793) / 2 : cvs.height - 793;
    ctx = cvs.getContext("2d");
    if (window.innerWidth < 500) hero.dX = 100;
    loop();
  });

  function onResize() {
    cvs.width = window.innerWidth - 18;
    cvs.height = window.innerHeight;
    yOffset = cvs.height > 793 ? (cvs.height - 793) / 2 : cvs.height - 793;
    if (window.innerWidth < 500) hero.dX = 100;
  }

  window.addEventListener("resize", onResize);

  function handleKeydown(event) {
    // console.log(event.keyCode)

    event.preventDefault();
    if (event.keyCode === 39 && event.keyCode !== lastPressedKey) {
      aminationDirection = RIGHT;
      isMoving = true;
      changeHeroAnimation(RUN);
    } else if (event.keyCode === 37 && event.keyCode !== lastPressedKey) {
      aminationDirection = LEFT;
      isMoving = true;
      changeHeroAnimation(RUN);
    } else if (event.keyCode === 40 && event.keyCode !== lastPressedKey) {
      if (hero.defaultAnimationType === RUN) {
        changeHeroAnimation(SLIDE);
      } else {
        changeHeroAnimation(CROUCH);
      }
    } else if (event.keyCode === 38 && event.keyCode !== lastPressedKey) {
      changeHeroAnimation(JUMP_2);
    }
    lastPressedKey = event.keyCode;
  }

  function handleKeyup(event) {
    if (event.keyCode === 39 && aminationDirection === "right") {
      isMoving = false;
      changeHeroAnimation(IDLE);
    } else if (event.keyCode === 37 && aminationDirection === "left") {
      isMoving = false;
      changeHeroAnimation(IDLE);
    }

    if (event.keyCode === lastPressedKey) lastPressedKey = -1;
  }

  const defaultHeroAnimationsList = [IDLE, IDLE_2, RUN];

  function changeHeroAnimation(type) {
    hero.frame = 0;
    if (defaultHeroAnimationsList.includes(type))
      hero.defaultAnimationType = type;
    hero.animationType = type;
  }

  function toggleDemo(isPlaying) {
    hero.frame = 0;
    if (isPlaying) {
      // heroAminationType = IDLE;
      isMoving = false;
      changeHeroAnimation(IDLE);
    } else {
      // heroAminationType = RUN;
      isMoving = true;
      changeHeroAnimation(RUN);
    }
  }
</script>

<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} />

<div class="game-wrapper">
  <canvas id="game-canvas" class="game" height="793" width="100" />

  <div class="touch-buttons">
    <button
      on:click={() => {
        changeHeroAnimation(SLIDE);
      }}
    />
    <button
      on:click={() => {
        changeHeroAnimation(JUMP);
      }}
    />
  </div>

  <DemoButtons {isMoving} {hero} {changeHeroAnimation} />

  <br />
  <!-- <YouTube0 /> -->
  <div><BackgroundPlayer {toggleDemo} /></div>
</div>

<style>
  .game-wrapper {
    position: relative;
    text-align: left;
    background: #0d1222;
  }

  .touch-buttons {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    display: flex;
  }
  .touch-buttons button {
    margin: 0;
    flex-grow: 1;
    border: none;
    background: none;
    outline: none;
    cursor: default;
  }

  .game {
    /* height: 793px; */
    /* height: 500px;
    width: 500px; */
    /* border: 1px solid red; */
    /* margin: 1rem; */
  }
</style>
