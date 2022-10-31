<template>
  <div class="consent-box">
    <div class="sections">
      <div class="section" :style="sectionStyle">
        <LineButton @click="showConsent" 
          >Interaktive Experience Beginnen</LineButton
        >
      </div>
      <div class="section">
        <DatenschutzButton @consented="consented" />
      </div>
    </div>
  </div>
</template>

<script>
import LineButton from "@/components/LineButton.vue";
import DatenschutzButton from "@/components/DatenschutzButton.vue";
export default {
  components: {
    LineButton,
    DatenschutzButton,
  },
  data() {
    return {
      moveBy: 0,
    };
  },
  computed: {
    sectionStyle() {
      return {
        "margin-left": "calc(-100vw * " + this.moveBy + " )",
        transition:
          "margin-left 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        "transition-timing-function":
          "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
      };
    },
  },
  methods: {
    showConsent: function () {
      this.moveBy = 1;
    },
    consented: function () {
      this.$emit("consented");
    },
  },
};
</script>

<style lang="scss">
.consent-box {
  background-color: var(--color-light);
  width: 100vw;
  overflow: hidden;
  flex-shrink: 0;
  .sections {
    width: 200vw;
    display: flex;
    .section {
      width: 100vw;
    }
  }
}
</style>
