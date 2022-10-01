<template>
  <nav>
    <router-link to="/">Home</router-link>
  </nav>
  <router-view/>
</template>

<style lang="scss">
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #666;
}

.button {
  background: rgb(127, 243, 243);
  cursor: pointer;
  width: auto;
  padding: .5em 2em;
  margin: .5rem;
  border-radius: 10em;
}

.uploadField{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: stretch ;
  max-width: 40em;

  margin: auto;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;

    &.router-link-exact-active {
      color: #00F;
    }
  }
}

//////////////////////////// EDITOR //////////////////////////////

@import "node_modules/vue-advanced-cropper/dist/theme.compact.scss";
.preview-editor {
  position: relative;
  width: 33vw;
  height: 33vw;
  // padding-bottom: 160%;
  .cropper {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    // height: 600px;
    // width: 600px;
    background: #ddd;
  }
}
</style>
