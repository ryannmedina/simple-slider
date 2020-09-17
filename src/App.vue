<template>
  <div id="app">
    
    <Slider />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Slider from './components/Slider.vue';

@Component({
  components: {
    Slider,
  },
})
export default class App extends Vue {
  
}
</script>

<style lang="scss">
body { 
  margin: 0 0;
  background-color: #ccc;
  overflow: hidden;

  #app {}
}
</style>
