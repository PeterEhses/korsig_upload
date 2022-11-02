<template>
  <div class="blocker" :style="styles.componentLevel">
    <div class="wrongRotationText" v-if="wrongAspect">
        <p>Bitte drehen Sie Ihr Gerät, sodass es hochkant anzeigt!</p>
    </div>
    <div class="wrongType" v-if="!md.phone() && !noPhoneIgnored">
      <BackgroundGraphic />
      <div class="wrongTypeText">
        <p>
          Ihr Gerät scheint kein Smartphone zu sein. Wollen Sie es trotzdem
          probieren? Diese Anwendung ist für Smartphones entwickelt, kann aber
          auch auf Tablets und PCs mit 16:9 hochkant Bildschirm funktionieren.
        </p>
        <LineButton @click="tryAnyway">Trotzdem probieren</LineButton>
      </div>
    </div>
    
  </div>
</template>

<script>
import { ref, computed, onMounted, onUnmounted } from "vue";

import BackgroundGraphic from "@/components/BackgroundGraphic.vue";
import LineButton from "@/components/LineButton.vue";
import MobileDetect from "mobile-detect";
export default {
  components: {
    BackgroundGraphic,
    LineButton,
  },
  setup() {
    //phone detect
    const md = new MobileDetect(window.navigator.userAgent);
    const deactivated = ref(false);
    if (md.phone()){
        deactivated.value = true
    }
    const noPhoneIgnored = ref(false);
    const tryAnyway = () => {
      noPhoneIgnored.value = true;
      deactivated.value = true;
    };

    // aspect ratio
    // get reactive width height
    const windowWidth = ref(window.innerWidth);
 const windowHeight = ref(window.innerHeight);
    const onWidthChange = () => {
        windowWidth.value = window.innerWidth
        windowHeight.value = window.innerHeight
    };
    onMounted(() => window.addEventListener("resize", onWidthChange));
    onUnmounted(() => window.removeEventListener("resize", onWidthChange));
        //compute aspect
    const aspect = computed(()=>{
        return(windowWidth.value/windowHeight.value)
    })
    const wrongAspect = computed(()=>{
        return(aspect.value > (3/3.5))
    })

    // add some fresh styles
    const styles = computed(() => {
      return {
        componentLevel: {
          display: (deactivated.value && !wrongAspect.value)  ? "none" : "inherit",
        },
      };
    });
    return {
      md,
      deactivated,
      noPhoneIgnored,
      tryAnyway,
      styles,
      aspect,
      wrongAspect
    };
  },
};
</script>

<style lang="scss">
.blocker {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  .wrongType {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-content: stretch;
    .wrongTypeText {
      min-height: 50%;
      padding: 2rem;
      background: var(--color-light);
      color: var(--color-dark);
      font-size: 2rem;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  }
    .wrongRotationText{
        position: absolute;
        width: 100%;
        height: 100%;
        padding: 2rem;
      background: var(--color-light);
      color: var(--color-dark);
      font-size: 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }
}
</style>
