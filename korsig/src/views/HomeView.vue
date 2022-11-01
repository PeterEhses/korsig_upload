<template>
  <div class="home">
    <!-- <UploadField @imageloaded="uploadFile" :status="uploadStatus" /> -->
    <!-- <p>
    Your UID is <code>{{this.$route.query.uid}}</code>
   </p> -->
    <div class="sent-status controls-section">
      <div class="upper-right-text status upload-status">
        {{ uploadStatus }}
      </div>
      <div class="info-section">
        <p class="upper-right-text" :style="sectionStyles.nextStepsHint">
          Die interaktive Experience geht auf dem Touch Terminal weiter…
        </p>
      </div>
    </div>
    <div
      class="upload-controls controls-section"
      :style="sectionStyles.editorControls"
      v-if="imageUrl.value != ''"
    >
      <div class="info-section">
        <p class="upper-right-text">
          Rotieren Sie Ihr Bild nach Belieben, schienden Sie es zu und skalieren
          Sie auf das Endformat der Projektion, indem Sie unten auf dem Bild mit
          einem beziehungsweise zwei Fingern ziehen.
        </p>
      </div>
      <UploadImageButton @rotate="rotate(90)" @upload="sendFileAndLock" />
    </div>
    <div class="imageload controls-section" :style="sectionStyles.select">
      <div class="info-section">
        <p class="upper-right-text">
          Laden Sie ein Bild hoch, um dieses öffentlich zur Installation
          beizutragen. Ihr Bild wird auf der Projektion erscheinen.
        </p>
      </div>
      <SelectImageButton
        @imageselected="imageSelected"
        @status="selectStatusChanged"
      />
    </div>
    <div class="start-section" :style="sectionStyles.start">
      <BackgroundGraphic class="bg-art" />
      <ConsentBox @consented="goToUpload" />
    </div>
    <div class="cropper-wrapper" :style="sectionStyles.editor">
      <div class="loading">
        {{ selectStatus }}
      </div>
      <div class="cropper-container" v-if="imageUrl">
        <cropper
          class="cropper"
          ref="cropArea"
          :stencil-props="{
            handlers: {},
            movable: false,
            scalable: false,
          }"
          :stencil-size="{
            width: 1200,
            height: 1920,
          }"
          image-restriction="stencil"
          :src="imageUrl"
          :moveImage="!sentUpload"
          :resizeImage="!sentUpload"
        ></cropper>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { useRoute } from "vue-router";
import { ref, computed } from "vue";
// import UploadField from "@/components/UploadField.vue";
import BackgroundGraphic from "../components/BackgroundGraphic.vue";
import ConsentBox from "@/components/ConsentBox.vue";
import SelectImageButton from "@/components/SelectImageButton.vue";
import UploadImageButton from "@/components/UploadImageButton.vue";
import { Cropper } from "vue-advanced-cropper";
import "vue-advanced-cropper/dist/style.css";
export default {
  name: "HomeView",
  components: {
    // UploadField,
    BackgroundGraphic,
    ConsentBox,
    SelectImageButton,
    UploadImageButton,
    Cropper,
  },
  setup() {
    // section animation logic
    const section = ref(0);
    const goToUpload = () => {
      section.value = 1;
    };
    const sectionStyles = computed(() => {
      return {
        start: {
          display: uploadStatus.value === "Hochladen erfolgreich!" ? "none" : "",
          top: "calc((100vh - 100vw) * " + (section.value > 0 ? 1 : 0) + " )",
          transition: "top 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
          "transition-timing-function":
            "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        },
        select: {
          opacity: imageUrl.value ? 0 : 1,
          "pointer-events": imageUrl.value ? "none" : "inherit",
          transition: "opacity 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
          "transition-timing-function":
            "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        },
        editorControls: {
          opacity: sentUpload.value ? 0 : 1,
          "pointer-events": sentUpload.value ? "none" : "inherit",
          transition: "opacity 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
          "transition-timing-function":
            "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        },
        editor: {
          display: selectStatus.value != "" ? "inherit" : "none",
          opacity: uploadStatus.value === "Hochladen erfolgreich!" ? 0 : 1,
          transition: "opacity 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
          "transition-timing-function":
            "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        },
        nextStepsHint: {
          opacity: uploadStatus.value === "Hochladen erfolgreich!" ? 1 : 0,
          transition: "opacity 1000ms cubic-bezier(1.000, 0.025, 0.665, 1.010)",
          "transition-timing-function":
            "cubic-bezier(1.000, 0.025, 0.665, 1.010)",
        }
      };
    });

    const imageUrl = ref("");
    // select image logic
    const imageSelected = (data) => {
      imageUrl.value = data;
      section.value = 2;
    };

    //edit image logic
    const cropArea = ref(null);
    const rotate = (angle) => {
      if (cropArea.value) {
        cropArea.value.rotate(angle);
      }
    };
    const sendFileAndLock = () => {
      const croppedImage = cropArea.value.getResult();
      croppedImage.canvas.toBlob((blob) => {
        uploadFile(blob);
      });
      sentUpload.value = true;
    };
    // Upload file logic

    const sentUpload = ref(false);

    const uploadStatus = ref("");
    const fetchResponse = ref({});
    const fetchError = ref({});
    const route = useRoute();
    const uploadFile = (data) => {
      uploadStatus.value = "Am hochladen...";
      let formData = new FormData();

      if (route.query && route.query.uid) {
        formData.append("uid", route.query.uid);
      }
      formData.append("file", data);

      fetch("https://3cw24m40.directus.app/files", {
        method: "POST",
        body: formData,
      }).then(
        function (response) {
          if (response.status != 204) {
            fetchError.value = response.status;
            uploadStatus.value = "Etwas ist schief gelaufen: " + String(fetchError.value);
          } else {
            uploadStatus.value = "Hochladen erfolgreich!";
            try {
              response.text().then(function (data) {
                const json = data === "" ? {} : JSON.parse(data);
                fetchResponse.value = json;
              });
            } catch (err) {
              console.warn(err);
              uploadStatus.value = "Etwas ist schief gelaufen: " + String(err);
            }
          }
        }.bind(this)
      );
    };

    // get and set upload status
    const selectStatus = ref("");
    const selectStatusChanged = (data) => {
      selectStatus.value = data;
    };

    return {
      section,
      goToUpload,
      sectionStyles,
      uploadFile,
      sentUpload,
      uploadStatus,
      fetchResponse,
      fetchError,
      imageUrl,
      imageSelected,
      selectStatus,
      selectStatusChanged,
      rotate,
      cropArea,
      sendFileAndLock,
    };
  },
};
</script>

<style lang="scss">
// general

.home {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.controls-section {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  left: 0;
  width: 100vw;
  height: calc(100vh - 100vw);
  background-color: var(--color-light);
  display: flex;
  flex-direction: column;
  align-content: stretch;
  .info-section {
    flex-grow: 1;
  }
  .upper-right-text {
      text-align: right;
      padding: 2rem;
      padding-bottom: 0;
      max-width: 35ch;
      margin-left: auto;
      &.status {
        font-size: 2rem;
      }
    }
}
.start-section {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

// cropper style sheet

@import "node_modules/vue-advanced-cropper/dist/theme.compact.scss";

.cropper-wrapper {
  position: absolute;
  overflow: hidden;
  width: 100vw;
  height: 100vw;
  left: 0;
  bottom: 0;
  right: 0;
  .loading {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--color-dark);
    color: var(--color-light);
  }
  .cropper-container {
    position: relative;
    width: 100%;
    height: 100%;
    .cropper {
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
    }
  }
}
</style>
