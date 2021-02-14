<script>
  import { onMount } from "svelte";
  import { getHeroAnimation } from "./animations";
  import YouTube0 from "./YouTube0.svelte";
  import BackgroundPlayer from "./BackgroundPlayer.svelte";

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
    RIGHT,
    LEFT,
  } from "./constants";

  let isMoving = false;
  let aminationDirection = RIGHT;
  let heroAminationType = IDLE;
  let frames = 0;
  let yOffset = 0;

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
      if (this.dy === 0) this.dy = yOffset;

      foregroundLayers.forEach((layer) => {
        ctx.drawImage(layer.img, this[layer.xName] - 2 * this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName] - this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName], this.dy);
        ctx.drawImage(layer.img, this[layer.xName] + this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName] + 2 * this.w, this.dy);
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
        heroAminationType !== RUN &&
        heroAminationType !== IDLE &&
        heroAminationType !== IDLE_2 &&
        heroAminationType !== CROUCH
      )
        heroAminationType = IDLE;
    },

    update: function () {
      this.animation = getHeroAnimation(heroAminationType, hero.w, hero.h);

      switch (heroAminationType) {
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
    loop();
  });

  function handleKeydown(event) {
    event.preventDefault();

    if (event.keyCode === 39) {
      aminationDirection = RIGHT;
      isMoving = true;
      heroAminationType = RUN;
    } else if (event.keyCode === 37) {
      heroAminationType = RUN;
      aminationDirection = LEFT;
      isMoving = true;
    }
  }

  function handleKeyup(event) {
    if (event.keyCode === 39 && aminationDirection === "right") {
      isMoving = false;
      heroAminationType = IDLE;
    } else if (event.keyCode === 37 && aminationDirection === "left") {
      isMoving = false;
      heroAminationType = IDLE;
    }
  }

  function changeHeroAnimation(type) {
    hero.frame = 0;
    heroAminationType = type;
  }

  function toggleDemo(isPlaying) {
    hero.frame = 0;
    if (isPlaying) {
      heroAminationType = IDLE;
      isMoving = false;
    } else {
      heroAminationType = RUN;
      isMoving = true;
    }
  }
</script>

<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} />

<div class="game-wrapper">
  <canvas id="game-canvas" class="game" height="793" width="100" />

  <div class="buttons">
    <button
      on:click={() => {
        isMoving = heroAminationType === IDLE;
        // changeHeroAnimation(heroAminationType === IDLE ? RUN : IDLE);
        if (heroAminationType === IDLE) {
          // playVideo()
          changeHeroAnimation(RUN);
        } else {
          // stopVideo()
          changeHeroAnimation(IDLE);
        }
      }}
    >
      {isMoving ? "stop" : "start"}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(RUN);
      }}
    >
      {RUN}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(IDLE);
      }}
    >
      {IDLE}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(IDLE_2);
      }}
    >
      {IDLE_2}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(JUMP);
      }}
    >
      jump
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(SLIDE);
      }}
    >
      slide
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(ATTACK);
      }}
    >
      attack
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(ATTACK_2);
      }}
    >
      {ATTACK_2}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(ATTACK_3);
      }}
    >
      {ATTACK_3}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(ATTACK_4);
      }}
    >
      {ATTACK_4}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(ATTACK_5);
      }}
    >
      {ATTACK_5}
    </button>

    <button
      on:click={() => {
        changeHeroAnimation(CROUCH);
      }}
    >
      {CROUCH}
    </button>
  </div>

  <hr />
  <YouTube0 />
  <div><BackgroundPlayer {toggleDemo} /></div>
</div>

<style>
  .game-wrapper {
    position: relative;
    text-align: left;
    background: black;
  }
  .buttons {
    /* position: absolute;
    bottom: 0; */
    display: flex;
    max-width: 100%;
    overflow: auto;
    width: 100%;
  }

  .buttons button {
    margin-right: 2px;
  }

  .game {
    /* height: 793px; */
    /* height: 500px;
    width: 500px; */
    /* border: 1px solid red; */
    /* margin: 1rem; */
  }
</style>
