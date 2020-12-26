<template>
  <div>
    <b-button type="is-primary m-3" @click="sendFiles()">
      Test sending files
    </b-button>
    <b-field class="is-primary">
      <b-upload v-model="dropFiles" multiple drag-drop>
        <section class="section is-primary">
          <div class="content has-text-centered is-primary">
            <p>
              <b-icon icon="upload" size="is-large" class="is-primary">
              </b-icon>
            </p>
            <p>Drop your files here or click to upload</p>
          </div>
        </section>
      </b-upload>
    </b-field>
    <div class="columns is-multiline">
      <div class="column is-one-quarter-desktop">
        <div v-for="file in createUrls()" :key="file" class="">
          <div class="card m-2">
            <div class="card-image">
              <img :src="file" class="uploaded-images image" alt="" />
            </div>
            <div class="card-content is-overlay is-clipped image-area">
              <b-button
                type="is-danger"
                class="image-tag"
                icon-right="delete"
                @click="removeFile(0)"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// let downloadjs = require("download.js");
export default {
  data() {
    return {
      dropFiles: []
    };
  },
  methods: {
    createUrls() {
      let urls = [];
      this.dropFiles.forEach(file => {
        urls.push(URL.createObjectURL(file));
      });

      return urls;
    },
    removeFile(index) {
      this.dropFiles.pop(index);
    },
    sendFiles() {
      let fd = new FormData();
      fd.append("file", this.dropFiles[0]);

      fetch("http://127.0.0.1:5000/transform", {
        method: "POST",
        headers: {
          Origin: ""
        },
        body: fd
      })
        .then(response => {
          console.log(response);
          return response.blob();
        })
        .then(data => {
          console.log(data);
          this.dropFiles.push(data);
          // downloadjs.downloadBlob("", data);
        })
        .catch(error => console.log(error));
    }
  }
};
</script>

<style lang="scss" scoped>
.uploaded-images {
  object-fit: cover;
}

.image-area {
  &:hover {
    .image-tag {
      opacity: 1;
    }
  }
}
.image-tag {
  opacity: 0;
  transition: 0.2s ease-in-out;
  /* adds this button to the top left corner*/
  position: absolute;
  left: 2vw;
  top: 2vw;
}
</style>
