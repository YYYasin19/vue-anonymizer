<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <FileUploader></FileUploader>
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import FileUploader from "../components/FileUploader.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
    FileUploader
  }
};
</script>
