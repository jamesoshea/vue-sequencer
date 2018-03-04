<template>
  <div class="sequencer__container">
    <div class="sequencer__header">
      <h1 class="sequencer__title">
        hello
      </h1>
      <h1 class="sequencer__message">
        i am sequencer
      </h1>
    </div>
    <Instrument
      v-for="instrument in instruments"
      :beat="beat"
      :isPlaying="isPlaying"
      :key="instrument.name"
      :name="instrument.name"
      :showName="instrument.showName"
      :src="instrument.src"
      :volume="instrument.volume"
      :notes="instrument.notes"
    />
    <div>
      <p>
        <button @click="play">
          PLAY
        </button>
        <button @click="stop">
          STOP
        </button>
      </p>
    </div>
    <div>
      <input
        class="sequencer__tempo-slider"
        type="range"
        min="40"
        max="200"
        step="1"
        v-model="tempo"
      />
      <p>
        Tempo: 
      </p>
      <input
        type="number"
        min="40"
        max="200"
        step="1"
        v-model="tempo"
      >
      <!-- <div>
        Tempo: {{tempo}}
      </div> -->
    </div>
  </div>
</template>

<script>
import Instrument from './Instrument.vue'
import patterns from '../assets/patterns'

export default {
  name: "Sequencer",
  components: {
    Instrument,
  },
  data() {
    return {
      instruments: [
        { 
          name: 'kick',
          showName: 'Kick',
          notes: patterns.samba.kick.notes,
          volume: 1,
          src: 'kick.wav'
        },
        {
          name: 'snare',
          showName: 'Snare',
          notes: patterns.samba.snare.notes,
          volume: 1,
          src: 'snare.wav'
        },
        {
          name: 'hats',
          showName: 'Hihat',
          notes: patterns.samba.closedHats.notes,
          volume: 1,
          src: 'hats.wav'
        },
        {
          name: 'openHats',
          showName: 'Open Hihat',
          notes: patterns.samba.openHats.notes,
          volume: 1,
          src: 'openHats.wav'
        },
        {
          name: 'crash',
          showName: 'Crash',
          notes: patterns.samba.crash.notes,
          volume: 1,
          src: 'crash.wav' },
      ],
      tempo: 120,
      beat: -1,
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
      clearInterval(this.player)
      document.querySelectorAll('audio').forEach(instrument => {
        instrument.pause()
        instrument.currentTime = 0
      })
      this.isPlaying = false
      this.beat = -1
    },
  },
  mounted() {
    document.addEventListener('keydown', event => {
      if(event.keyCode !== 32) {
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

<style lang="scss" scoped>
@import '../assets/main.scss';

.sequencer__container {
  width: 80%;
  margin: auto;
}

.sequencer__header {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.sequencer__title {
  font-family: 'Roboto', sans-serif;
  font-size: 6em;
  margin: 0;
}

.sequencer__message {
  font-family: 'Roboto', sans-serif;
  color: $dark-vanilla;
  font-size: 2em;
  margin: 0;
  transform: rotate(24deg) translateY(20%);
}

.sequencer__tempo-slider {
  position: relative;
  top: 50%;
}

input[type=range] {
  -webkit-appearance: none;
  &:focus {
    outline: none;
  }
  &::-webkit-slider-runnable-track {
    height: 1em;
    -webkit-appearance: none;
    background: $dark-vanilla;
  }
  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    height: 1em;
    width:1em;
    background: $spanish-bistre;
  }
}

</style>
