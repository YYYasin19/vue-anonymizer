<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <div class="container">
      <div class="columns">
        <div class="column is-three-quarters">
          <FileUploader class="box"></FileUploader>
        </div>
        <div class="column">
          <div class="box">
            <div class="text">Here is the other column</div>
          </div>
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
