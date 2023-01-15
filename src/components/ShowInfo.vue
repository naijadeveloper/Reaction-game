<script>
  export default {
    name: "ShowInfo",
    props: ['time', 'mood', 'color', 'aboutInfo'],
    methods: {
      playAgain() {
        this.$emit('playAgain');
      },
      closeAbout() {
        this.$emit(this.$refs.aboutme.textContent)
      }
    }
  }
</script>


<template>
  <div class="alert-backdrop">
    <div class="alert-box" :style="color">
      <slot name="timing">
        <h1>Snail</h1>
        <p>It took you {{ time }} miliseconds to identify your mood and select it.</p>
        <p>Are you sure that you are truly <span>{{ mood == 'nothing'? "feeling nothing": mood }}</span>?</p>
      </slot>
      <button v-if="!aboutInfo && time < 1300" @click="playAgain">Thank you</button>
      <button v-else-if="!aboutInfo && time >= 1300" @click="playAgain">Go away</button>
      <button ref="aboutme" class="close-about" v-else @click="closeAbout">OK</button>
    </div>
  </div>
</template>


<style>
  .alert-backdrop {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    
    display: flex;
    justify-content: center;
    align-items: center;

    padding: 25px;
    background-color: rgb(68, 68, 68, 0.95)
  }

  .alert-box {
    color: #444;
    background-color: #eee;
    padding: 10px;
    border-radius: 5px;
  }

  .alert-box h1 {
    position: relative;
    overflow: hidden;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    text-transform: uppercase;
    font-family: calibri;
    font-size: 28px;
  }

  .alert-box h1:after {
    content: "";
    flex: 2;
    width: 180px;
    height: 3px;
    background: var(--bg-color);
  }

  .alert-box p {
    font-family: calibri;
  }

  .alert-box p:nth-child(3) {
    margin-bottom: 10px;
  }

  .alert-box span {
    border-bottom: 2px solid var(--bg-color);
  }

  .alert-box button {
    margin: 5px auto;
    background-color: var(--bg-color);
    color: white;
    border: none;
    border-radius: 3px;
    width: 70px;
    height: 30px;
    transition: opacity 0.5s;
    cursor: pointer;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  .alert-box button.close-about {
    background-color: #444;
  }
</style>