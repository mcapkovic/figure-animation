<script>
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
  export let isMoving;
  export let hero;
  export let changeHeroAnimation;


</script>

<div class="buttons">
  <button
    on:click={() => {
      isMoving = hero.animationType === IDLE;
      // changeHeroAnimation(heroAminationType === IDLE ? RUN : IDLE);
      if (hero.animationType === IDLE) {
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

<style>
 
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
  

  </style>
  