<template>
  <div class="home">
    <!-- <UploadField @imageloaded="uploadFile" :status="uploadStatus" /> -->
   <!-- <p>
    Your UID is <code>{{this.$route.query.uid}}</code>
   </p> -->
   <BackgroundGraphic class="bg-art"/>
   <div class="templightbg"><LineButton>Interaktive Experience Beginnen</LineButton></div>
  </div>
</template>

<script>
// @ is an alias to /src
import { useRoute } from 'vue-router';
import { ref } from "vue";
// import UploadField from "@/components/UploadField.vue";
import BackgroundGraphic from '../components/BackgroundGraphic.vue';
import LineButton from '@/components/LineButton.vue';
export default {
  name: "HomeView",
  components: {
    // UploadField,
    BackgroundGraphic,
    LineButton
  },
  setup() {
    const uploadStatus = ref("waiting for upload...");
    const fetchResponse = ref({});
    const fetchError = ref({});
    const route = useRoute();
    const uploadFile = (data) => {
      uploadStatus.value = "Uploading...";
      let formData = new FormData();

      
      if(route.query && route.query.uid){
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
            uploadStatus.value = "Error: " + String(fetchError.value);
          } else {
            uploadStatus.value = "Upload successful";
            try {
              response.text().then(function (data) {
                const json = data === "" ? {} : JSON.parse(data);
                fetchResponse.value = json;
              });
            } catch (err) {
              console.warn(err)
            }
          }
        }.bind(this)
      );
    };
    return {
      uploadFile,
      uploadStatus,
      fetchResponse,
      fetchError,
    };
  },
};
</script>

<style lang="scss">
.home{
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>