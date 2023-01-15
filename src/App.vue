<script>
  import Block from "./components/Block.vue";
  import ShowInfo from "./components/ShowInfo.vue";

  export default {
    name: "App",
    components: { Block, ShowInfo },
    data() {
      return {
        isPlaying: false,
        selectedMood: null,
        selectedColor: '',
        countdown: 5,
        timeSpent: null,
        aboutInfo: false
      }
    },
    methods: {
      play() {
        //Start 5sec countdown
        if(this.selectedMood) {
          this.isPlaying = true;
          let counting = setInterval(() => {
            if(this.countdown == 0) {
              clearInterval(counting);
            }else {
              this.countdown--;
            }
          }, 1000);
        }
      },
      selectMood(mood, color) {
        if(!this.isPlaying) {
          if(this.selectedMood == mood) {
            this.selectedMood = null;
            this.selectedColor = "";
          }else {
            this.selectedMood = mood;
            this.selectedColor = color;
          }
        }
      },
      respond(timeSpent) {
        this.timeSpent = timeSpent;
        // this.isPlaying = false;
        // this.selectedMood = null;
        // this.selectedColor = '';
        // this.countdown = 5;
      },
      playAgain() {
        this.isPlaying = false;
        this.selectedMood = null;
        this.selectedColor = '';
        this.countdown = 5;
        this.timeSpent = null;
      },
      openAbout() {
        this.aboutInfo = true;
      },
      closeAbout() {
        this.aboutInfo = false;
      }
    }
  }
</script>

<template>
  <!--colors
    darkorange-happy, crimson-angry, darkorchid-sad, slateblue-sleepy, #444-nothing 
  -->
  <div class="info">
    <h1 style="text-decoration:underline; margin-bottom: 20px;">REACTION GAME</h1>
    <p class="description">You have five moods (
      <span @click="selectMood('happy', '--bg-color: darkorange')" class="happy" :class="{'span-border': selectedMood == 'happy'}">Happy😁</span>

      <span @click="selectMood('angry', '--bg-color: crimson')" class="angry" :class="{'span-border': selectedMood == 'angry'}">Angry😡</span>

      <span @click="selectMood('sad', '--bg-color: darkorchid')" class="sad" :class="{'span-border': selectedMood == 'sad'}">Sad😟</span>

      <span @click="selectMood('sleepy', '--bg-color: slateblue')" class="sleepy" :class="{'span-border': selectedMood == 'sleepy'}">Sleepy😴</span>
      and 
      <span @click="selectMood('nothing', '--bg-color: #444')" class="nothing" :class="{'span-border': selectedMood == 'nothing'}">Nothing😶</span>)
    </p>
    <p v-if="!selectedMood">SELECT ONE!!!</p>
    <p v-else>You have Selected <span>{{ selectedMood }}</span>. The emoji for your mood will be in one of five corners below, how fast can you identify it and select it?</p>
    <button :disabled="isPlaying" @click="play" class="play">Play</button>
    <p v-if="isPlaying && countdown > 0">Game starts in {{ countdown }}</p>
  </div>

  <Block v-if="isPlaying && countdown == 0" :mood="selectedMood" @react="respond" />

  <ShowInfo v-if="timeSpent" :time="timeSpent" :mood="selectedMood" :color="selectedColor" @playAgain="playAgain">
    <template v-if="timeSpent < 1300" v-slot:timing>
      <h1>Cheetah</h1>
      <p>It took you {{ timeSpent }} miliseconds to identify your mood and select it.</p>
      <p>You are truly <span>{{ selectedMood == 'nothing'? "feeling nothing": selectedMood }}</span></p>
    </template>
  </ShowInfo>


  <div v-if="!isPlaying" class="about-btn" @click="openAbout">About me</div>
  <teleport to="#about">
    <ShowInfo v-if="aboutInfo" :aboutInfo="aboutInfo" @OK="closeAbout">
      <template v-slot:timing>
        <h1>About</h1>
        <p>Built for your pleasure by <a href="https://naijadeveloper-github-info-website.vercel.app/" target="_blank">Enoch Enujiugha</a></p>
      </template>
    </ShowInfo>
  </teleport>
</template>

<style>
  .info {
    padding: 5px;
  }

  .info p.description, p.description + p, p.description + p + p {
    margin-top: 8px;
  }

  .info span, .about-btn {
    display: inline-block;
    padding: 5px;
    margin: 3px;
    border-radius: 3px;
    background-color: #444;
    text-transform: capitalize;
    cursor: pointer;
    user-select: none;
  }

  .span-border {
    border: 3px solid pink;
  }

  span.happy {
    background-color: darkorange;
  }

  span.angry {
    background-color: crimson;
  }

  span.sad {
    background-color: darkorchid;
  }

  span.sleepy {
    background-color: slateblue;
  }

  button.play {
    margin: 10px auto;
    background-color: seagreen;
    color: white;
    border: none;
    border-radius: 3px;
    width: 70px;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: opacity 0.5s;
  }

  button[disabled] {
    background-color: #777;
    cursor: not-allowed;
  }

  button:hover {
    opacity: 0.7;
  }

  a {
    text-decoration: underline;
    font-family: calibri;
  }

  .about-btn {
    position: fixed;
    top: 10px;
    right: 10px;
    font-size: 15px
  }
</style>
