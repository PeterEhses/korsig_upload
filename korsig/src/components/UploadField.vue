<template>
  <div class="uploadField">
    <div @click="sendEventImageSelect()" class="button">
      <p>Choose a file to art with</p>
    </div>
    <input
      style="display: none"
      ref="EventfileInput"
      @change="onFileChanged"
      type="file"
      name="upload"
      accept="image/*"
    />
    <div class="editor-wrapper" v-if="imageUrl">
      <div class="preview-editor" >
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
    <div class="editor-controls" v-if="!sentUpload"><div class="button" @click="rotate(90)">Rotate</div>
    <div class="button" @click="emitFileAndLock()">Upload</div></div>
    <div class="status" v-else>
      <p>{{ status }}</p>
    </div>
    </div>
    
    
    
  </div>
</template>

<script>
import { ref } from "vue";
import { Cropper } from "vue-advanced-cropper";
import 'vue-advanced-cropper/dist/style.css';

export default {
  name: "UploadField",
  props: {
    status: String,
  },
  components: { cropper: Cropper },
  emits: ["imageloaded"],
  // eslint-disable-next-line
  setup(props, context) {
    const imageUrl = ref("");
    const sentUpload = ref(false);
    const EventfileInput = ref(null);
    const cropArea = ref(null);
    const onFileChanged = (event) => {
      const files = event.target.files;
      const image = files[0];
      console.log(image);
      const filename = files[0].name;
      if (filename.lastIndexOf(".") <= 0) {
        return alert("No files selected");
      }
      const fileReader = new FileReader();
      fileReader.addEventListener("load", () => {
        imageUrl.value = fileReader.result;
        // context.emit("imageloaded", files[0]);
      });
      fileReader.readAsDataURL(files[0]);
    };
    const sendEventImageSelect = () => {
      EventfileInput.value.click();
    };

    const rotate = (angle)  => {
      cropArea.value.rotate(angle)
    }

    const emitFileAndLock = () => {
      const croppedImage = cropArea.value.getResult()
      croppedImage.canvas.toBlob((blob) => {context.emit("imageloaded", blob)})
      sentUpload.value = true
    }
    return {
      imageUrl,
      sendEventImageSelect,
      onFileChanged,
      EventfileInput,
      cropArea,
      rotate,
      emitFileAndLock,
      sentUpload
    };
  },
};
</script>

<style lang="scss">
.uploadField{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: stretch ;
  max-width: 40em;
  margin: auto;
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