<template>
  <div id="slider">
    <div v-for="slide in slides" 
      :key="slide.id" 
      :class="{
        'slide': true, next: slide.next, current: slide.current, previous: slide.previous,
        'slide-in': slide.current && slideDirection,
        'slide-out': slide.previous && slideDirection,
        'slide-in-backwards': slide.previous && !slideDirection,
        'slide-out-backwards': slide.current && !slideDirection        
      }"
      @mouseenter="paused = true"
      @mouseover="paused = true"
      @mouseleave="paused = false"
    >
      <component :is="slide.component" />
    </div>
    <div id="next-button" @mouseenter="paused = true" @mouseover="paused = true" @mouseleave="paused = false"><a href="#" @click="next">next</a></div>
    <div id="previous-button" @mouseenter="paused = true" @mouseover="paused = true" @mouseleave="paused = false"><a href="#" @click="previous">previous</a></div>
    <ul id="menu">
      <li v-for="slide in slides" 
        :key="slide.id + '-link'"
        @mouseenter="paused = true"
        @mouseover="paused = true"
        @mouseleave="paused = false"
      >  
        <a href="#" @click="goTo(slide.id)" :class="{ current: slide.current}">{{slide.id}}</a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Slide1 from './slides/Slide1.vue'
import Slide2 from './slides/Slide2.vue'
import Slide3 from './slides/Slide3.vue'


@Component({
  components: {
    Slide1, Slide2, Slide3
  },
})


export default class Slider extends Vue {
  // Data
  public time: number = 2000; 
  
  public slides: Array<any> = [
    { id: 'slide-1', component: Slide1, next: false, current: false, previous: true },
    { id: 'slide-2', component: Slide2, next: false, current: true, previous: false},
    { id: 'slide-3', component: Slide3, next: true, current: false, previous: false },
    { id: 'slide-4', component: Slide1, next: false, current: false, previous: false },
    { id: 'slide-5', component: Slide3, next: false, current: false, previous: false },
    { id: 'slide-6', component: Slide2, next: false, current: false, previous: false }
  ]
  
  public paused: boolean = false
  // true is forward and false is backwards
  public slideDirection: boolean = true;

  // Computed
  get slideCount(): number { return this.slides.length; }
  get currentIndex(): number { return this.slides.findIndex(slide => slide.current) }

  // Methods
  timer() {
    if (!this.paused) {
      this.next()
    }
  }

  next(slide ?: number) {
      let newSlides = this.resetSlides()

      let newCurrent = (this.currentIndex + 1 === this.slideCount) ? 0 : this.currentIndex + 1
      
      if (Number.isInteger(slide) && slide !== undefined) { newCurrent = slide }
      
      let newNext = (newCurrent + 1 === this.slideCount) ? 0 : newCurrent + 1      
      let newPrevious = (newCurrent === 0) ? this.slideCount - 1 : newCurrent - 1

      newSlides[newNext].next = true
      newSlides[newCurrent].current = true
      newSlides[newPrevious].previous = true

      this.slides = newSlides;
      this.slideDirection = true
  }

  previous(slide ?: number) {
      let newSlides = this.resetSlides()

      let newCurrent = (this.currentIndex === 0) ? this.slideCount - 1 : this.currentIndex - 1
      
      if (Number.isInteger(slide) && slide !== undefined) { newCurrent = slide }
      
      let newNext = (newCurrent === 0) ? this.slideCount - 1 : newCurrent - 1      
      let newPrevious = (newCurrent + 1 === this.slideCount) ? 0 : newCurrent + 1  

      newSlides[newNext].next = true
      newSlides[newCurrent].current = true
      newSlides[newPrevious].previous = true

      this.slides = newSlides;
      this.slideDirection = false
  }

  resetSlides() {
      return this.slides.map(slide => {
        let newSlide = {...slide}

        newSlide.next = false;
        newSlide.current = false;
        newSlide.previous = false;

        return newSlide;
      })
  }

  goTo(slideKey: string) {
    
    const slideIndex = this.slides.findIndex(slide => slide.id === slideKey)

    if (slideIndex < this.currentIndex) {
      this.previous(slideIndex)
    } if (slideIndex > this.currentIndex) {
      this.next(slideIndex)
    }

  }

  mounted() {
    setInterval(this.timer, this.time)
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
  100% { left: 100vw; display: none; }
}

@keyframes slide-in-backwards {
  0%   { left: 0; }
  100% { left: -100vw; display: none; }
}

@keyframes slide-out-backwards {
  0%   { left: 100vw; }
  100% { left: 0; }
}

#slider {
  position: relative;
  width: 100vw;
  max-width: 100%;
  margin: 0 auto;
  
  .slide { 
    width: 100vw;
    position: absolute;
    left: -100vw;
    display: block;
    font-size: 0;

    &.next { left: -100vw; }
      
    &.current { left: 0; }

    &.previous { left: 100vw; }
    
    &.slide-in { animation-name: slide-in; animation-duration: 500ms; animation-iteration-count: 1; animation-timing-function: ease-out; }
    &.slide-in-backwards { animation-name: slide-in-backwards; animation-duration: 500ms; animation-iteration-count: 1; animation-timing-function: ease-out; }

    &.slide-out { animation-name: slide-out; animation-duration: 500ms; animation-iteration-count: 1; animation-timing-function: ease-out; }
    &.slide-out-backwards { animation-name: slide-out-backwards; animation-duration: 500ms; animation-iteration-count: 1; animation-timing-function: ease-out; }    

    div {
      
      img {
        max-width: 100%;
        
      }
    }
  }

  #previous-button, #next-button {
    position: absolute;
    z-index: 2;
    margin-top: 25%;
  }

    #previous-button { left: 20px; }
    #next-button { right: 20px; }

  #menu { 
    position: absolute; 
    margin-left: 35%;
    z-index: 2; 
    list-style: none; 

    li {
      display: inline-block; margin-right: 10px; 

      .current { color: #fff; }

    }
  }



}

</style>
