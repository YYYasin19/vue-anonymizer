<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <FileUploader></FileUploader>
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
  computed: {}
};
</script>
