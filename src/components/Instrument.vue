<template>
  <div class="instrument__row">
    <div
      class="instrument__title"
      v-text="name"
    />
    <div class="instrument__volume">
      <input
        type="range"
        min="0"
        max="1"
        step="0.1"
        v-model="localVolume"
      />
    </div>
    <div class="instrument__beats">
      <div
        v-for="(note, i) in notes"
        :key="note.beat"
        :class="{
          [i]: true,
          'instrument__beat--current-beat': isCurrentBeat(i),
          'instrument__beat--downbeat': isDownbeat(i)
        }"
      >
        <input type="checkbox" v-model="notes[i].value">
      </div>
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
      isDownbeat(beat) {
        return beat % 4 === 0 
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
  flex-grow: 1;
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

</style>
