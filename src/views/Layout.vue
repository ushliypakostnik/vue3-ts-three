<template>
  <div class="layout">
    <Scene />
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>Welcome to<br />Vue.js 3 + TypeScript + Three.js + Stylus<br />App Starter!!!</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Scene from '@/components/Scene.vue';

export default defineComponent({
  name: 'Layout',

  components: {
    Scene,
  },
});
</script>

<style scoped lang="stylus">
// @import "~/src/stylus/_stylebase.styl";

.layout
  position fixed
  top 0
  left 0
  right 0
  bottom 0
  width 100vw
  height: 100vh

  img
    margin-top 10vh
    position relative

h1
  font-size 40px
  position relative
  color #ffffff
</style>
