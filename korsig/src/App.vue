<template>
  <router-view/>
</template>

<style lang="scss">
@import "the-new-css-reset/css/reset.css";
@import url("https://use.typekit.net/hjc1xqc.css");

html{
  font-size: 16px;
}

body{

  --color-dark: #1C2020;
  --color-light: #C2C1B8;
  color: var(--color-dark);
  background-color: var(--color-dark);


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

--easing-function: cubic-bezier(1.000, 0.025, 0.665, 1.010);
}

@media screen and (max-height: 666px) {
  html{
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
  padding: .5em 2em;
  margin: .5rem;
  border-radius: 10em;
}

a{
  text-decoration: underline;
}
</style>
