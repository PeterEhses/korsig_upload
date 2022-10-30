<template>
  <div class="line-button-wrapper"><div class="line-button"><slot></slot></div></div>
</template>

<script>
export default {

}
</script>

<style lang="scss">
.line-button-wrapper{
    padding: 2rem;
}
.line-button{
    word-spacing: 100vw;
    font-size: 2rem;
    text-align: right;
    border-right: .175rem solid var(--color-dark);
    padding: 0 1.5rem;
}
</style>
