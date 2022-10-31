<template>
  <div>
    <LineButton @click="sendEventImageSelect()">Bilddatei auswählen</LineButton>
    <input
      style="display: none"
      ref="EventfileInput"
      @change="onFileChanged"
      type="file"
      name="upload"
      accept="image/*"
    />
  </div>
</template>

<script>
import LineButton from "@/components/LineButton.vue";
import { ref, watch } from "vue";
export default {
  components: {
    LineButton,
  },
  emits: ["imageselected", "status"],
    // eslint-disable-next-line
  setup(props, context) {
    const imageUrl = ref("");
    const EventfileInput = ref(null);
    const status = ref("");

    const onFileChanged = (event) => {
      status.value = "Bild lädt..."
      const files = event.target.files;
    //   const image = files[0];
    //   console.log(image);
      const filename = files[0].name;
      if (filename.lastIndexOf(".") <= 0) {
        status.value = "Keine Dateien ausgewählt!"
        return alert("No files selected");
      }
      const fileReader = new FileReader();
      fileReader.addEventListener("load", () => {
        imageUrl.value = fileReader.result;
        context.emit("imageselected", fileReader.result);
        status.value = "Bitte warten..."
        // context.emit("imageloaded", files[0]);
      });
      fileReader.readAsDataURL(files[0]);
    };

    watch(status, () => {
      context.emit("status", status.value)
    })

    const sendEventImageSelect = () => {
      EventfileInput.value.click();
    };
    return {
      status,
      imageUrl,
      EventfileInput,
      onFileChanged,
      sendEventImageSelect,
    };
  },
};
</script>

<style></style>
