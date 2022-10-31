<template>
  <div class="upload-buttons-wrapper">
    <LineButton :ltr="true" @click="$emit('upload')">Bild absenden</LineButton>
    <RotateButton @click="$emit('rotate')"/>
  </div>
</template>

<script>
import LineButton from "@/components/LineButton.vue";
import RotateButton from "@/components/RotateButton.vue";
export default {
    components: {
    LineButton,
    RotateButton
  },
  emits: [
    'upload',
    'rotate'
  ]
}
</script>

<style lang="scss">
.upload-buttons-wrapper{
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>