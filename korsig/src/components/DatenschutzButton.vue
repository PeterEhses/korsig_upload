<template>
  <div class="ds-button">
    <div class="text">
      <p>
        "Ich bin Einverstanden mit der
        <a @click="toggleLegalText">Verwendung meines Bildes</a>."
      </p>
    </div>
    <div class="ja">
      <img
        src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7"
      />
      <div class="ja-inner" @click="consentYes"><p>ja</p></div>
    </div>
    <div v-if="legalVisible" class="legal-text">
        <LegalText/>
        <div class="go-back" @click="toggleLegalText">Zurück zur Experience</div>
    </div>
  </div>
</template>

<script>
import LegalText from "@/components/LegalText.vue"
export default {
    components: {
        LegalText,
    },
  data() {
    return {
      consented: false,
      legalVisible: false,
    };
  },
  methods: {
    consentYes: function () {
      this.consented = true;
      this.$emit("consented");
    },
    toggleLegalText: function () {
      this.legalVisible = !this.legalVisible;
    },
  },
};
</script>

<style lang="scss">
.ds-button {
  background-color: var(--color-dark);
  color: var(--color-light);
  height: calc(100% - 4rem);
  margin: 2rem;
  border-radius: 9000px;
  display: flex;
  align-content: stretch;
  .text {
    flex-shrink: 1;
    flex-grow: 1;
    margin: 1rem 0rem 1rem 2rem;
    color: inherit;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .ja {
    position: relative;
    height: calc(100% - 3em);
    flex-shrink: 0;
    margin: 1.5em;
    background-color: var(--color-light);
    color: var(--color-dark);

    border-radius: 9999px;
    img {
      display: block;
      height: 100%;
      width: auto;
    }
    .ja-inner {
      position: absolute;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
      p {
        font-size: 2.5rem;
        text-align: center;
        margin-top: -0.333rem;
        margin-right: -0.333rem;
      }
    }
  }
}

.legal-text {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: var(--color-dark);
  color: var(--color-light);
    transform: unset;
    display: flex;
    flex-direction: column;
    .go-back{
        flex-shrink: 0;
        min-height: 6rem;
        margin: 2rem;
        font-size: 1.5rem;
        background-color: var(--color-light);
        color: var(--color-dark);
        text-align: center;
        display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 9999px;
    }
}
</style>
