<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <div class="container">
      <div class="columns">
        <div class="column">
          <FileUploader class="m-3"></FileUploader>
        </div>
      </div>
    </div>

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
    return {};
  },
  computed: {}
};
</script>

<style lang="scss" scoped>
img {
  width: 30%;
}
</style>
