<template>
  <div class="instrument__row">
    <div
      class="instrument__title"
      v-text="showName"
    />
    <div class="instrument__volume">
      <input
        class="instrument__volume-input"
        type="range"
        min="0"
        max="1"
        step="0.1"
        v-model="localVolume"
      />
    </div>
    <div class="instrument__pattern">
      <select v-model="selectedStyle">
        <option
          v-for="style in patternStyles"
          :key="style"
          :value="style"
          :label="style"
        />
      </select>
    </div>
    <div class="instrument__beats">
      <div
        v-for="(note, i) in patterns[selectedStyle][name].notes"
        :key="note.beat"
        :class="{
          [i]: true,
          'instrument__beat--current-beat': isCurrentBeat(i),
          'instrument__beat--downbeat': isDownbeat(i)
        }"
      >
        <input
          type="checkbox"
          v-model="patterns[selectedStyle][name].notes[i].value"
          :class="isChecked(patterns[selectedStyle][name].notes[i].value)"
        >
      </div>
    </div>
    <audio
      :data-sound="name"
      :src="src"
    />
  </div>
</template>

<script>
import patterns from '../assets/patterns'

export default {
  props: {
    isPlaying: Boolean,
    beat: Number,
    src: String,
    name: String,
    showName: String,
    volume: [String, Number],
  },
  data() {
    return {
      patterns,
      localVolume : this.volume,
      selectedStyle: 'samba'
    }
  },
  computed: {
    notes() {
      return this.patterns[this.selectedStyle][this.name].notes
    },
    patternStyles() {
      return Object.keys(this.patterns)
    }
  },
  watch: {
    localVolume(newValue) {
      document.querySelector(`[data-sound="${this.name}"]`).volume = newValue
    },
    beat(newValue) {
      if (newValue < 0) {
        return
      }
      if (this.notes[newValue].value) {
        document.querySelector(`[data-sound="${this.name}"]`).pause()
        document.querySelector(`[data-sound="${this.name}"]`).currentTime = 0
        document.querySelector(`[data-sound="${this.name}"]`).play()
      }
    },
  },
  methods: {
    isCurrentBeat(beat) {
      return this.isPlaying && this.beat === beat 
    },
    isDownbeat(beat) {
      return beat % 4 === 0 
    },
    isChecked(value) {
      return {
        'instrument__note': true,
        'instrument__note--checked': value
      }
    },
  },
}
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';

.instrument__row {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}

.instrument__title {
  flex-basis: 15%;
  text-align: left;
}

.instrument__volume {
  flex-grow: 0.25;
}

.instrument__pattern {
  flex-grow: 0.5;
}

.instrument__beats {
  flex-grow: 3;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.instrument__beat--current-beat {
  background-color: $dark-vanilla;
}

.instrument__beat--downbeat {
  border-left: 1px solid $darkest-grey;
}

.instrument__note {
  &:focus {
    outline: none;
  }
  position: relative;
  top:50%;
  transform: translateY(-75%);
  appearance: none;
  border: 1px solid $spanish-bistre;
  height: 1em;
  width: 1em;
}

.instrument__note--checked {
  background-color: $spanish-bistre;
}

input[type=range] {
  -webkit-appearance: none;
  position: relative;
  top:50%;
  transform: translateY(-62.5%);
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
