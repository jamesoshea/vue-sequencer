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
          class="instrument-block"
          v-for="(note, j) in instrument.notes"
          :key="note.beat"
          :class="{ [j]: true }"
        >
          <input type="checkbox" v-model="instruments[i].notes[j].value">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
      kit: 1
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
  
}

.instrument-block {

}

</style>
