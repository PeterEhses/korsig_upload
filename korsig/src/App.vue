<template>
  <router-view v-if='!over'/>
  <WrongDeviceBlocker v-if='!over'/>
  <OverMessage v-if='over'/>
</template>

<script>
import WrongDeviceBlocker from "@/components/WrongDeviceBlocker.vue";
import OverMessage from '@/components/OverMessage.vue'
export default {
  components: {
    WrongDeviceBlocker,
    OverMessage
  },
  data(){
    return{
      over: true
    }
  }
};
</script>


<style lang="scss">
@import "the-new-css-reset/css/reset.css";
@import url("https://use.typekit.net/hjc1xqc.css");

html {
  font-size: 16px;
}

body {
  --color-dark: #1c2020;
  --color-light: #c2c1b8;
  color: var(--color-dark);
  background-color: var(--color-light);

  font-family: museo-sans, sans-serif;
  font-weight: 500;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;

  overflow: hidden;
  /* assign vw and vh as variable in case we want to suppress rotation */
  /* height is greater than width */
  --large-dim: 1vh;
  --small-dim: 1vw;

  /* animation stuff */

  --easing-function: cubic-bezier(1, 0.025, 0.665, 1.01);
}

#app{
  width: 100%;
  height: 100%;
}

@media screen and (max-height: 666px) {
  html {
    font-size: 12px;
  }
}
// @media screen and (max-height: 630px) {
//   html{
//     font-size: 10px;
//   }
// }

@media screen and (min-width: 100vh) {
  /* width is greater than height */
  body {
    --large-dim: 1vw;
    --small-dim: 1vh;
  }
}

.button {
  background: var(--color-dark);
  color: var(--color-light);
  cursor: pointer;
  width: auto;
  padding: 0.5em 2em;
  margin: 0.5rem;
  border-radius: 10em;
}

a {
  text-decoration: underline;
}

.over-message{
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
