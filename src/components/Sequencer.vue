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
      :key="instrument.name"
      :isPlaying="isPlaying"      
      :beat="beat"
      :src="instrument.src"
      :name="instrument.name"
      :showName="instrument.showName"
      :volume="instrument.volume"
      :globalPatternStyle="globalPatternStyle"
    />
    <div class="sequencer__tempo-container">
      <span>How fast?</span>   
      <input
        type="number"
        min="40"
        max="200"
        step="1"
        v-model="tempo"
      >
    </div>
    <div class="sequencer__transport-container">
      <p>
        <button @click="play">
          PLAY
        </button>
        <button @click="stop">
          STOP
        </button>
      </p>
    </div>
    <div class="sequencer__global-pattern-style">
      <select v-model="globalPatternStyle">
        <option 
          v-for="patternStyle in patternStyles"
          :key="patternStyle"
          :label="patterns[patternStyle].showName"
          :value="patternStyle"
        />
      </select>
    </div>
  </div>
</template>

<script>
import patterns from '../assets/patterns'
import Instrument from './Instrument.vue'

export default {
  name: "Sequencer",
  components: {
    Instrument,
  },
  data() {
    return {
      patterns,
      globalPatternStyle: 'samba',
      instruments: [
        { 
          name: 'kick',
          showName: 'Kick',
          volume: 1,
          src: 'kick.wav'
        },
        {
          name: 'snare',
          showName: 'Snare',
          volume: 1,
          src: 'snare.wav'
        },
        {
          name: 'closedHats',
          showName: 'Hihat',
          volume: 1,
          src: 'hats.wav'
        },
        {
          name: 'openHats',
          showName: 'Open Hihat',
          volume: 1,
          src: 'openHats.wav'
        },
        {
          name: 'crash',
          showName: 'Crash',
          volume: 1,
          src: 'crash.wav' },
      ],
      tempo: 120,
      beat: -1,
      player: null,
      isPlaying: false
    };
  },
  computed: {
    patternStyles() {
      return Object.keys(patterns)
    }
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

.sequencer__tempo-container {
  text-align: center;
  margin-top: 1em;
}

.sequencer__transport-container {
  text-align: center;
  margin-top: 1em;
}

.sequencer__global-pattern-style {
  text-align: center;
  margin-top: 1em;
}

input[type=number] {
  &::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  border: 1px solid $spanish-bistre;
  border-radius: 2px;
  background-color: $lightest-grey;
  color: $darkest-grey;
  text-align: center;
}

</style>
