<template>
  <div id="slide-2">
    <img src="../../assets/slide-2.jpg" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Slide2 extends Vue {
  // Data

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  #slide-2 {
    
    img {
      max-width: 100%;
      
    }
  }

</style>
