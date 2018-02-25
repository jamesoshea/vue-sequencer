<template>
  <div class="container">
    <div class="title-row">
      <p>
        wow i am a sequencer
      </p>
    </div>
    <div v-for="(instrument, i) in instruments" :key="instrument.name">
      <div class="instrument-row">
        <div
          class="instrument-title"
          v-text="instrument.name"
        />
        <div
          class="instrument-beat"
          v-for="(note, j) in instrument.notes"
          :key="note.beat"
          :class="{ [j]: true }"
        >
          <input type="checkbox" v-model="instruments[i].notes[j].value">
        </div>
      </div>
    </div>
    <div>
      <button @click="play">
        PLAY
      </button>
      <button @click="stop">
        STOP
      </button>
    </div>
    <audio data-sound="kick" src="../../Kick.wav"></audio>
    <audio data-sound="snare" src="../../Snare.wav"></audio>
  </div>
</template>

<script>
let player; 

export default {
  name: "HelloWorld",
  data() {
    return {
      instruments: [
        { name: 'kick', notes: noteGrid() },
        { name: 'snare', notes: noteGrid() },
        { name: 'hats', notes: noteGrid() },
        { name: 'crash', notes: noteGrid() },
      ],
      tempo: 120,
      kit: 1,
      beat: 1,
      player: null
    }
  },
  methods: {
    play() {
      this.beat = 1
      this.player = setInterval(() => {
        if (this.beat === 5) this.beat = 1
        this.instruments.forEach((instrument) => {
          if (!instrument.notes[this.beat - 1].value) {
            return;
          }
          document.querySelector(`[data-sound="${instrument.name}"]`).pause()
          document.querySelector(`[data-sound="${instrument.name}"]`).currentTime = 0
          document.querySelector(`[data-sound="${instrument.name}"]`).play()
        })
        this.beat += 1
      }, 60000 / this.tempo)
    },
    stop() {
      this.beat = 1
      clearInterval(this.player)
    }
  }
};

function noteGrid() {
  return [
    {beat: 1, value: false},
    {beat: 2, value: false},
    {beat: 3, value: false},
    {beat: 4, value: false},
  ]
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container {
  width: 80%;
  height: 200px;
  margin: auto;
}

.instrument-row {
  display: flex;
  flex-direction: row;
}

.instrument-title {
  width: 30%;
  text-align: center;
}

.instrument-beat {
  width: 15%;
}

</style>
