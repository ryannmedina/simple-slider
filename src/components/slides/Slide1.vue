<template>
  <div><img :src="getImgUrl(slide.src)" /></div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Slide1 extends Vue {
  // Data
  public time: number = 2000; 
  
  public slides: Array<any> = [
    { id: 'slide-1', src: 'slide-1', next: false, current: false, previous: true },
    { id: 'slide-2', src: 'slide-2', next: false, current: true, previous: false},
    { id: 'slide-3', src: 'slide-3', next: true, current: false, previous: false },
    { id: 'slide-4', src: 'slide-1', next: false, current: false, previous: false },
    { id: 'slide-5', src: 'slide-3', next: false, current: false, previous: false },
    { id: 'slide-6', src: 'slide-2', next: false, current: false, previous: false }
  ]
  
  public paused: boolean = false
  // true is forward and false is backwards
  public slideDirection: boolean = true;

  // Computed
  get slideCount(): number { return this.slides.length; }
  get currentIndex(): number { return this.slides.findIndex(slide => slide.current) }


  getImgUrl(image: string) {
    var images = require.context('../assets/', false, /\.jpg$/)
    return images('./' + image + ".jpg")
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">


</style>
