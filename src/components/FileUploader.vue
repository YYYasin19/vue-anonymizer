<template>
  <div>
    <b-button type="is-primary m-3" @click="sendFiles3()">
      Test sending files
    </b-button>
    <b-field class="is-primary">
      <b-upload multiple drag-drop @input="newFiles" native>
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
        <div v-for="(image, index) in images" :key="index" class="">
          <div class="card m-2">
            <b-loading
              :is-full-page="false"
              v-model="image.loading"
              :can-cancel="true"
            ></b-loading>
            <div class="card-image">
              <img :src="image.fileurl" class="uploaded-images image" alt="" />
            </div>
            <div class="card-content is-overlay is-clipped image-area">
              <b-button
                type="is-danger"
                class="image-tag"
                icon-right="delete"
                @click="removeFile(index)"
              />
            </div>
            <div class="card-footer">
              {{ image.name }}
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
      dropFiles: [],
      images: []
    };
  },
  methods: {
    newFiles(files) {
      files.forEach(file => {
        this.images.push({
          fileurl: URL.createObjectURL(file),
          file: file,
          name: file.name,
          loading: false
        });
      });
    },
    createUrls() {
      let urls = [];
      this.dropFiles.forEach(file => {
        urls.push({
          fileurl: URL.createObjectURL(file),
          name: file.name
        });
      });

      return urls;
    },
    removeFile(index) {
      this.images.splice(index, 1);
    },
    async sendFiles() {
      let fd;

      // loop over images and send request for every one of them
      let index = 0;
      for (var elem of this.images) {
        fd = new FormData();
        fd.append("file", elem.file);
        elem.loading = true;

        fetch("http://127.0.0.1:5000/transform", {
          method: "POST",
          headers: {
            Origin: "" // set our origin here, so only this app is allowed
          },
          body: fd
        })
          .then(response => response.blob())
          .then(blobData => {
            // replace images at index 0 with new image
            this.$set(this.images, index, {
              fileurl: URL.createObjectURL(blobData),
              file: blobData,
              name: "Processed: " + elem.name,
              loading: false
            });
          })
          .catch(error => console.log(error));

        index++;
      }
    },

    async sendFiles3() {
      Promise.all(
        this.images.map((elem, index) => {
          let fd = new FormData();
          fd.append("file", elem.file);

          return fetch("http://127.0.0.1:5000/transform", {
            method: "POST",
            headers: {
              Origin: "" // set our origin here, so only this app is allowed
            },
            body: fd
          })
            .then(response => response.blob())
            .then(blobData => {
              // replace images at index 0 with new image
              this.$set(this.images, index, {
                fileurl: URL.createObjectURL(blobData),
                file: blobData,
                name: "Processed: " + elem.name
              });
            })
            .catch(error => console.log(error));
        })
      );
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
