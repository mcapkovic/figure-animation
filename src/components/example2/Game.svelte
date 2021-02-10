<script>
  import { onMount } from "svelte";

  const IDLE = "idle";
  const IDLE2 = "idle2";
  const SLIDE = "slide";
  const RUN = "run";
  const CROUCH = "crouch";
  const ATTACK = "attack";
  const JUMP = "jump";
  const RIGHT = "right";
  const LEFT = "left";

  let isMoving = false;
  let aminationDirection = RIGHT;
  let frames;

  // SELECT CVS
  let cvs;
  let ctx;

  // LOAD IMAGE
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
      ctx.drawImage(layer_0009, 0, cvs.height - 793);
      ctx.drawImage(
        layer_0009,
        this.sX,
        this.sY,
        this.w,
        this.h,
        this.dx + this.w,
        cvs.height - 793,
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
      if (this.dy === 0) this.dy = cvs.height - 793;

      foregroundLayers.forEach((layer) => {
        ctx.drawImage(layer.img, this[layer.xName] - 2 * this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName] - this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName], this.dy);
        ctx.drawImage(layer.img, this[layer.xName] + this.w, this.dy);
        ctx.drawImage(layer.img, this[layer.xName] + 2 * this.w, this.dy);
      });
    },

    update: function () {
      //   if(state.current == state.game){
      //       this.x = (this.x - this.dx)%(this.w/2);
      //   }
      //   this.dx = (this.dx - 5) % this.w;

      if (isMoving)
        foregroundLayers.forEach((layer) => {
          this[layer.xName] =
            aminationDirection === RIGHT
              ? (this[layer.xName] - layer.increment) % this.w
              : (this[layer.xName] + layer.increment) % this.w;
        });
    },
  };

  // DRAW
  function draw() {
    ctx.fillStyle = "#7693b3";
    ctx.fillRect(0, 0, cvs.width, cvs.height);
    // bg.draw();
    // pipes.draw();
    fg.draw();
    // bird.draw();
    // getReady.draw();
    // gameOver.draw();
    // score.draw();
  }

  // UPDATE
  function update() {
    // bird.update();
    fg.update();
    // pipes.update();
  }

  // LOOP
  function loop() {
    update();
    draw();
    frames++;
    console.log(frames);
    requestAnimationFrame(loop);
  }

  onMount(() => {
    cvs = document.querySelector("#game-canvas");
    cvs.width = window.innerWidth;
    cvs.height = window.innerHeight;
    ctx = cvs.getContext("2d");
    // console.log( cvs.width)
    // console.log( cvs.height)

    // console.log(layer_0006);
    loop();
  });

  function handleKeydown(event) {
    // key = event.key;
    // keyCode = event.keyCode;

    if (event.keyCode === 39) {
      aminationDirection = RIGHT;
      isMoving = true;
      //   animationChange(RUN);
      //   if (!moveBackroungID) moveBackroungID = requestAnimationFrame(moveX);
      // moveX()
    } else if (event.keyCode === 37) {
      // cancelAnimationFrame(moveBackroungID);
      // moveBackroungID=null;
      //   if (!moveBackroungID) moveBackroungID = requestAnimationFrame(moveX);

      aminationDirection = LEFT;
      isMoving = true;

      //   animationChange(RUN);
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
    if (event.keyCode === 39 && aminationDirection === "right") {
      isMoving = false;
    } else if (event.keyCode === 37 && aminationDirection === "left") {
      isMoving = false;
    }
    // else if(event.keyCode === 40){
    //     animationChange(IDLE)
    // }
  }
</script>

<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} />

<div>
  <canvas id="game-canvas" class="game" height="793" width="928" />
  <button on:click={() => (isMoving = !isMoving)}>
    {isMoving ? "stop" : "run"}</button
  >
</div>

<style>
  /* #game-canvas{
        height: 600;
    } */
  .game {
    /* height: 793px; */
    /* height: 500px;
    width: 500px; */
    /* border: 1px solid red; */
    /* margin: 1rem; */
  }
</style>
