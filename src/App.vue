<template>
  <BoardCmp @reset-cmp="(n) => (index += n)" :key="index"></BoardCmp>
</template>

<script>
import BoardCmp from './components/BoardCmp.vue';
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    BoardCmp,
  },
  setup() {
    const index = ref(1);
    return {
      index,
    };
  },
};
</script>

<style lang="scss">
/* @link https://utopia.fyi/type/calculator?c=375,35,1.2,1920,150,1.25,5,2,&s=0.75|0.5|0.25,1.5|2|3|4|6,s-l */

:root {
  --step--2: clamp(0.87rem, calc(0.59rem + 1.17vw), 2rem);
  --step--1: clamp(1.04rem, calc(0.69rem + 1.51vw), 2.5rem);
  --step-0: clamp(1rem, calc(0.44rem + min(11.44vw, 12vh)), 20rem);
  --step-1: clamp(1.5rem, calc(0.92rem + 2.49vw), 3.91rem);
  --step-2: clamp(1.8rem, calc(1.05rem + 3.19vw), 4.88rem);
  --step-3: clamp(2.16rem, calc(1.2rem + 4.08vw), 6.1rem);
  --step-4: clamp(2.59rem, calc(1.37rem + 5.22vw), 7.63rem);
  --step-5: clamp(3.11rem, calc(1.55rem + 6.66vw), 9.54rem);
}

body {
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100wv;
}
#app {
  font-family: sans-serif, Roboto, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: grid;
  align-content: center;
  gap: 20px;
  place-items: center;
  width: 100vw;
  height: 100vh;
  position: absolute;
  inset: 0;
}
</style>
