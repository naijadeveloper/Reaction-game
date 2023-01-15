<template>
  <div class="block-section">
    <div 
    @click="handleMooding" 
    class="top-left-corner" 
    :data-mood="allMoods[0]">
      {{ emojis[allMoods[0]] }}
    </div>

    <div 
    @click="handleMooding" 
    class="top-right-corner" 
    :data-mood="allMoods[1]">
      {{ emojis[allMoods[1]] }}
    </div>

    <div 
    @click="handleMooding" 
    class="middle-corner" 
    :data-mood="allMoods[2]">
      {{ emojis[allMoods[2]] }}
    </div>

    <div 
    @click="handleMooding" 
    class="bottom-left-corner" 
    :data-mood="allMoods[3]">
      {{ emojis[allMoods[3]] }}
    </div>

    <div 
    @click="handleMooding" 
    class="bottom-right-corner" 
    :data-mood="allMoods[4]">
      {{ emojis[allMoods[4]] }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Block",
  props: ['mood'],
  data() {
    return {
      allMoods: ['happy', 'angry', 'sad', 'sleepy', 'nothing'].sort(() => Math.random() - 0.5),// To get a random arrangement of the values in the array everytime? use the SORT method, to be honest I dont fully understand the logic behind it, but it works so....LEAVE IT!!
      emojis: { happy: '😁', angry: '😡', sad: '😟', sleepy: '😴', nothing: '😶' },
      timer: null,
      timeSpent: 0
    }
  },
  methods: {
    handleMooding(e) {
      let mood = e.target.dataset.mood;
      if(this.mood == mood) {
        clearInterval(this.timer);
        this.$emit("react", this.timeSpent)
      }
    },
  },
  mounted() {
    this.timer = setInterval(() => {
      this.timeSpent+=10;
    }, 10);
  }
}
</script>

<style>
  .block-section {
    border: 8px solid #333;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  div[class$='corner'] {
    width: 80px;
    height: 80px;
    font-size: 55px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #333;
    cursor: pointer;
  }

  div.top-left-corner {
    position: absolute;
    top: 0;
    left: 0;
  }

  div.top-right-corner {
    position: absolute;
    top: 0;
    right: 0;
  }

  div.bottom-left-corner {
    position: absolute;
    bottom: 0;
    left: 0;
  }

  div.bottom-right-corner {
    position: absolute;
    bottom: 0;
    right: 0;
  }

</style>