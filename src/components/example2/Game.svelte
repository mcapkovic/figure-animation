<script>
  import { onMount } from "svelte";

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
    x: 0,
    y: 0,

    draw: function () {
      // console.log(ctx)
      ctx.drawImage(layer_0009, 0, 0);

    //   ctx.drawImage(
    //     layer_0009,
    //     this.sX,
    //     this.sY,
    //     this.w,
    //     this.h,
    //     this.x,
    //     this.y,
    //     this.w,
    //     this.h
    //   );

      // ctx.drawImage(layer_0009, this.sX, this.sY, this.w, this.h, this.x + this.w, this.y, this.w, this.h);
    },
  };

  // DRAW
  function draw() {
    ctx.fillStyle = "#7693b3";
    ctx.fillRect(0, 0, cvs.width, cvs.height);
    bg.draw();
    // pipes.draw();
    // fg.draw();
    // bird.draw();
    // getReady.draw();
    // gameOver.draw();
    // score.draw();
  }

  // UPDATE
  function update() {
    // bird.update();
    // fg.update();
    // pipes.update();
  }

  // LOOP
  function loop() {
    update();
    draw();
    frames++;
    // console.log(frames)
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
</script>

<canvas id="game-canvas" class="game" height="793" width="928" />

<style>
  /* #game-canvas{
        height: 600;
    } */
  .game {
    /* height: 793px; */
    /* height: 500px;
    width: 500px; */
    border: 1px solid red;
    /* margin: 1rem; */
  }
</style>
