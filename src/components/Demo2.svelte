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
      const prevAnimation = aminationType
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

    function handleKeydown(event) {
		// key = event.key;
        // keyCode = event.keyCode;
        
        if(event.keyCode === 39){
            aminationDirection = 'right'
            animationChange(RUN)
        }else if(event.keyCode === 37){
            aminationDirection = 'left'
            animationChange(RUN)
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
        if(event.keyCode === 39 && aminationDirection === 'right' && aminationType === RUN){
            animationChange(IDLE)
        }else if( event.keyCode === 37 && aminationDirection === 'left' && aminationType === RUN){
            animationChange(IDLE)

        }
        // else if(event.keyCode === 40){
        //     animationChange(IDLE)
        // }
	}
  </script>

<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup}/>

  
  <div class="demo">
    <Hero {aminationType} {aminationDirection} />
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
    .demo {
      text-align: center;
      padding: 1em;
      margin: 0 auto;
    }
  </style>
  