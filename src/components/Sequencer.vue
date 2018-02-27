<template>
  <div class="container">
    <div class="title-row">
      <p>
        wow i am a sequencer
      </p>
    </div>
      <Instrument
        v-for="instrument in instruments"
        :beat="beat"
        :isPlaying="isPlaying"
        :key="instrument.name"
        :name="instrument.name"
        :src="instrument.src"
        :volume="instrument.volume"
        :notes="instrument.notes"
      />
    <div>
      <button @click="play">
        PLAY
      </button>
      <button @click="stop">
        STOP
      </button>
      <input type="range" min="40" max="200" step="1" v-model="tempo">
      Tempo: {{tempo}}
    </div>
  </div>
</template>

<script>
import Instrument from './Instrument.vue'

export default {
  name: "Sequencer",
  components: {
    Instrument,
  },
  data() {
    return {
      instruments: [
        { name: 'kick', notes: noteGrid(), volume: 1, src: 'kick.wav' },
        { name: 'snare', notes: noteGrid(), volume: 1, src: 'snare.wav' },
        { name: 'hats', notes: noteGrid(), volume: 1, src: 'hats.wav' },
        { name: 'openHats', notes: noteGrid(), volume: 1, src: 'openHats.wav' },
        { name: 'crash', notes: noteGrid(), volume: 1, src: 'crash.wav' },
      ],
      tempo: 120,
      beat: 15,
      player: null,
      isPlaying: false,
    };
  },
  methods: {
    play() {
      if (this.isPlaying) {
        return;
      }
      this.isPlaying = true
      this.player = setInterval(() => {
        this.beat += 1
        if (this.beat === 16) {
          this.beat = 0
        }
      }, 60000 / (this.tempo * 4))
    },
    stop() {
      this.isPlaying = false
      this.beat = 15
      clearInterval(this.player)
      document.querySelectorAll('audio').forEach(instrument => {
        instrument.pause()
        instrument.currentTime = 0
      })
    },
  },
  mounted() {
    document.addEventListener('keydown', event => {
      if(!(event.keyCode === 32)) {
        return
      }
      event.preventDefault()
      if (!this.isPlaying) {
        this.play()
      } else {
        this.stop()
      }
    })
  }
};

function noteGrid() {
  return [
    {beat: 0, value: false},
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
    {beat: 15, value: false}
  ]
}
</script>

<style scoped>

.container {
  width: 80%;
  height: 200px;
  margin: auto;
}

</style>
