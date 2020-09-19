<template>
  <div id="slide-1">
    <img src="../../assets/slide-1.jpg" />

    <div class="content">
        <h1>SPRING 2019</h1>
    </div>

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Slide1 extends Vue {
  // Data

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  #slide-1 {
    position: relative;
    
    img {
      max-width: 100%;
      z-index: 1;
    }

    .content {
      position: absolute;
      top: 0;
      width: 100%;
      margin: 20% auto 0 auto;
      z-index: 2;
      text-align: center;

      color: #fff;

      h1 { font-size: 30px; }

    }

  }

</style>
