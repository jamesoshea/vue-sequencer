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
          :class="{ [j]: true, currentBeat: isCurrentBeat(note.beat) }"
        >
          <input type="checkbox" v-model="instruments[i].notes[j].value">
        </div>
      </div>
    </div>
    <div>
      <button @click.prevent="play">
        PLAY
      </button>
      <button @click.prevent="stop">
        STOP
      </button>
      <input type="range" min="40" max="200" step="1" v-model="tempo">
      Tempo: {{tempo}}
    </div>
    <audio data-sound="kick" src="../../Kick.wav"></audio>
    <audio data-sound="snare" src="../../Snare.wav"></audio>
    <audio data-sound="hats" src="../../Hats.wav"></audio>
    <audio data-sound="crash" src="../../Crash.wav"></audio>
    <ul style="text-align:left;">
      <li>add spacebar support</li>
      <li>add samples for crash and hats</li>
      <li>fix highlighting for current beat</li>
      <li>styling</li>
    </ul>
  </div>
</template>

<script>
let player; 

export default {
  name: "Sequencer",
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
      player: null,
      isPlaying: false,
    }
  },
  methods: {
    play() {
      if (this.isPlaying) {
        return;
      }
      this.isPlaying = true
      this.beat = 1
      this.player = setInterval(() => {
        if (this.beat === 17) this.beat = 1
        this.instruments.forEach((instrument) => {
          if (!instrument.notes[this.beat - 1].value) {
            return;
          }
          document.querySelector(`[data-sound="${instrument.name}"]`).pause()
          document.querySelector(`[data-sound="${instrument.name}"]`).currentTime = 0
          document.querySelector(`[data-sound="${instrument.name}"]`).play()
        })
        this.beat += 1
      }, 60000 / (this.tempo * 4))
    },
    stop() {
      this.isPlaying = false
      this.beat = 1
      clearInterval(this.player)
      document.querySelectorAll('audio').forEach(instrument => {
        instrument.pause()
        instrument.currentTime = 0
      })
    },
    isCurrentBeat(beat) {
      return this.beat === beat
    }
  },
  mounted() {
    document.addEventListener('keydown', event => {
      if(!(event.keyCode === 32)) {
        return
      }
      if (!this.isPlaying) {
        console.log('wow')
        this.play()
      } else {
        this.stop()
      }
    })
  }
};

function noteGrid() {
  return [
    {beat: 1, value: false},
    {beat: 2, value: false},
    {beat: 3, value: false},
    {beat: 4, value: false},
    {beat: 5, value: false},
    {beat: 6, value: false},
    {beat: 7, value: false},
    {beat: 8, value: false},
    {beat: 9, value: false},
    {beat: 10, value: false},
    {beat: 11, value: false},
    {beat: 12, value: false},
    {beat: 13, value: false},
    {beat: 14, value: false},
    {beat: 15, value: false},
    {beat: 16, value: false}
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

.currentBeat {
  background-color: mediumseagreen;
}

</style>
