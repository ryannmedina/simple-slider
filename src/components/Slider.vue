<template>
  <div id="slider">
    <div v-for="slide in slides" 
      :key="slide.id" 
      :class="{next: slide.next, current: slide.current, previous: slide.previous}"
      @mouseenter="paused = true"
      @mouseleave="paused = false"
    >
      <div><img :src="getImgUrl(slide.src)" /></div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Slider extends Vue {
  // Data
  public slides: Array<any> = [
    { id: 'slide-1', src: 'slide-1', next: false, current: false, previous: true },
    { id: 'slide-2', src: 'slide-2', next: false, current: true, previous: false},
    { id: 'slide-3', src: 'slide-3', next: true, current: false, previous: false },
    { id: 'slide-4', src: 'slide-1', next: false, current: false, previous: false },
    { id: 'slide-5', src: 'slide-2', next: false, current: false, previous: false }
  ]
  
  public paused: boolean = false

  // Computed
  get slideCount(): number { return this.slides.length; }
  get currentIndex(): number { return this.slides.findIndex(slide => slide.current) }


  getImgUrl(image: string) {
    var images = require.context('../assets/', false, /\.jpg$/)
    return images('./' + image + ".jpg")
  }

  timer() {

    if (!this.paused) {
      let newSlides = this.slides.map(slide => {
        let newSlide = {...slide}

        newSlide.next = false;
        newSlide.current = false;
        newSlide.previous = false;

        return newSlide;
      })

      let newCurrent = (this.currentIndex + 1 === this.slideCount) ? 0 : this.currentIndex + 1
      let newNext = (newCurrent + 1 === this.slideCount) ? 0 : newCurrent + 1      
      let newPrevious = this.currentIndex

      newSlides[newNext].next = true
      newSlides[newCurrent].current = true
      newSlides[newPrevious].previous = true

      this.slides = newSlides;
    }
  }

  mounted() {

    setInterval(this.timer, 2000)
  

  }



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

@keyframes slide-in {
  0%   { left: -100vw; }
  100% { left: 0; }
}

@keyframes slide-out {
  0%   { left: 0; }
  100% { left: 100vw; }
}

#slider {
  position: relative;
  width: 100vw;
  max-width: 100%;
  margin: 0 auto;
  
  > div { 
    width: 100vw;
    position: absolute;
    left: -100vw;
    display: block;
    font-size: 0;

    &.next { left: -100vw; display: none; }
      
    &.current { 
      left: 0;
      animation-name: slide-in;
      animation-duration: 1s;
      animation-iteration-count: 1;
      animation-timing-function: ease-out;
    }

    &.previous {
      left: 100vw;
      animation-name: slide-out;
      animation-duration: 1s;
      animation-iteration-count: 1;
      animation-timing-function: ease-out;
    }

    div {
      
      img {
        max-width: 100%;
        
      }
    }
  }
}

</style>
