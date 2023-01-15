<script>
import Block from "./components/Block.vue";

export default {
  name: "App",
  components: { Block },
  data() {
    return {
      isPlaying: false,
      selectedMood: null,
      countdown: 5,
    }
  },
  methods: {
    play() {
      //Start 3sec countdown
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
    selectMood(mood) {
      if(!this.isPlaying) {
        if(this.selectedMood == mood) {
          this.selectedMood = null;
        }else {
          this.selectedMood = mood;
        }
      }
    },
    respond(timeSpent) {
      console.log(timeSpent);
      this.isPlaying = false;
      this.selectedMood = null;
      this.countdown = 5;
    }
  }
}
</script>

<template>
  <div class="info">
    <h1 style="text-decoration:underline; margin-bottom: 20px;">REACTION GAME</h1>
    <p class="description">You have five moods (
      <span @click="selectMood('happy')" class="happy" :class="{'span-border': selectedMood == 'happy'}">Happy😁</span>
      <span @click="selectMood('angry')" class="angry" :class="{'span-border': selectedMood == 'angry'}">Angry😡</span> 
      <span @click="selectMood('sad')" class="sad" :class="{'span-border': selectedMood == 'sad'}">Sad😟</span> 
      <span @click="selectMood('sleepy')" class="sleepy" :class="{'span-border': selectedMood == 'sleepy'}">Sleepy😴</span> and 
      <span @click="selectMood('nothing')" class="nothing" :class="{'span-border': selectedMood == 'nothing'}">Nothing😶</span>)</p>
    <p v-if="!selectedMood">SELECT ONE!!!</p>
    <p v-else>You have Selected <span>{{ selectedMood }}</span>. The emoji for your mood will be in one of five corners below, how fast can you identify it and select it?</p>
    <button :disabled="isPlaying" @click="play" class="play">Play</button>
    <p v-if="isPlaying && countdown > 0">Game starts in {{ countdown }}</p>
  </div>
  <Block v-if="isPlaying && countdown == 0" :mood="selectedMood" @react="respond" />
</template>

<style>
.info {
  padding: 5px;
}

.info p.description, p.description + p, p.description + p + p {
  margin-top: 8px;
}

.info span {
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
}

button[disabled] {
  background-color: #777;
  cursor: not-allowed;
}
</style>
