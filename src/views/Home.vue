<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <b-steps v-model="activeStep">
      <b-step-item step="1" label="Upload images" icon="folder-multiple-image">
        <FileUploader></FileUploader>
      </b-step-item>
      <b-step-item step="2" label="Select parameters" icon="tune">
        Things are here
      </b-step-item>
      <b-step-item step="3" label="Review results" icon="image-area-close">
        Other things too
      </b-step-item>
    </b-steps>
    <!--
<FileUploader></FileUploader>
    -->
  </div>
</template>

<script>
// @ is an alias to /src
import FileUploader from "../components/FileUploader.vue";

export default {
  name: "Home",
  components: {
    FileUploader
  },
  data() {
    return {
      activeStep: 0
    };
  },
  computed: {
    steps() {
      return [
        {
          label: "Upload images",
          content: FileUploader,
          type: "component"
        },
        {
          label: "Select parameters",
          content: "Here you can select parameters",
          type: "html"
        },
        {
          label: "Finish",
          content: "Here you can send your request",
          type: "html"
        }
      ];
    }
  }
};
</script>
