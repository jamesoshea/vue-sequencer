<template>
  <div id="app">
    <Sequencer/>
  </div>
</template>

<script>
import Sequencer from './components/Sequencer.vue'

export default {
  name: 'app',
  components: {
    Sequencer,
  },
}
</script>

<style lang="scss">
@import 'assets/main.scss';
</style>
