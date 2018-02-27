<template>
  <div class="instrument-row">
    <div
      class="instrument-title"
      v-text="name"
    />
    <div class="instrument-volume">
      <input type="range" min="0" max="1" step="0.1" v-model="localVolume">
    </div>
    <div
      class="instrument-beat"
      v-for="(note, i) in notes"
      :key="note.beat"
      :class="{ [i]: true, currentBeat: isCurrentBeat(i) }"
    >
      <input type="checkbox" v-model="notes[i].value">
    </div>
    <audio
      :data-sound="name"
      :src="src"
    />
  </div>
</template>

<script>
  export default {
    props: {
      isPlaying: Boolean,
      beat: Number,
      src: String,
      name: String,
      volume: [String, Number],
      notes: Array,
    },
    data() {
      return {
        localVolume: this.volume,
      }
    },
    watch: {
      localVolume(newValue) {
        document.querySelector(`[data-sound="${this.name}"]`).volume = newValue
      },
      beat(newValue) {
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
    }
  }
</script>

<style scoped>

.instrument-row {
  display: flex;
  flex-direction: row;
}

.instrument-title {
  width: 36%;
  text-align: left;
}

.instrument-beat {
  width: 4%;
}

.currentBeat {
  background-color: mediumseagreen;
}

</style>
