<template>
  <div :class="['line-button-wrapper', ltr ? 'ltr' : '']"><div class="line-button"><slot></slot></div></div>
</template>

<script>
export default {
  props: {
    ltr: Boolean
  }
}
</script>

<style lang="scss">
.line-button-wrapper{
overflow: hidden;   
&.ltr{
      .line-button{
        text-align: left;
      border-right: none;
      border-left: .175rem solid var(--color-dark);
      }
    } 
}
.line-button{
    word-spacing: 100vw;
    font-size: 2rem;
    text-align: right;
    border-right: .175rem solid var(--color-dark);
    padding: 0 1.5rem;
    margin: 2rem 2rem;
    

    &:hover{
        opacity: .6;
    }
}
</style>
