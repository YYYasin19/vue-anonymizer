<template>
  <div>
    <b-field class="is-primary">
      <b-upload multiple drag-drop @input="newFiles" native expanded>
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

    <b-button
      type="is-primary"
      @click="sendFiles3()"
      icon-right="send"
      expanded
    >
      Process current images
    </b-button>

    <!-- Image Area -->
    <!-- TODO: Split into component -->
    <div class="row columns is-multiline my-1">
      <div
        v-for="(image, index) in images"
        :key="index"
        class="column is-4 zoom-container"
      >
        <div class="card large image-card">
          <b-loading
            :is-full-page="false"
            v-model="image.loading"
            :can-cancel="true"
          ></b-loading>
          <b-button
            type="is-danger"
            class="delete-button tag is-small"
            icon-right="delete"
            @click="removeFile(index)"
          />
          <div class="card-image">
            <figure class="image is-16by9 ">
              <img :src="image.fileurl" alt="Image" />
            </figure>
          </div>
        </div>
      </div>
    </div>

    <!-- Image Area (End) -->

    <!--
    <div class="columns is-multiline">
      <div class="">
        <div v-for="(image, index) in images" :key="index" class="">
          <div class="card column is-one-quarter-desktop is-half-tablet">
            <b-loading
              :is-full-page="false"
              v-model="image.loading"
              :can-cancel="true"
            ></b-loading>
            <div class="card-image">
              <img :src="image.fileurl" class="uploaded-images image" alt="" />
            </div>
            <div class="card-content is-overlay is-clipped button-area">
              <b-button
                type="is-danger"
                class="delete-button"
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
    -->
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
          loading: false,
          processed: false
        });
      });
    },
    removeFile(index) {
      this.images.splice(index, 1);
    },

    async sendFiles3() {
      Promise.all(
        this.images.map((elem, index) => {
          if (elem.processed == false && elem.loading == false) {
            // create form data
            let fd = new FormData();
            fd.append("file", elem.file);

            // set current state to "loading" --> user feedback
            elem.loading = true;

            // return a promise that consists of the transformation request
            return fetch("http://127.0.0.1:5000/transform", {
              method: "POST",
              headers: {
                Origin: "" // set our origin here, so only this app is allowed
              },
              body: fd
            })
              .then(response => response.blob())
              .then(blobData => {
                console.log(blobData);
                // replace images at index 0 with new image
                this.$set(this.images, index, {
                  fileurl: URL.createObjectURL(blobData),
                  file: new File([blobData], elem.name),
                  name: "Processed: " + elem.name,
                  processed: true
                });
              })
              .catch(error => console.log(error));
          }
        })
      );
    },
    async sendFiles4() {
      Promise.all(
        this.images.map((elem, index) => {
          if (elem.processed == false) {
            let fd = new FormData();
            fd.append("file", elem.file);
            elem.loading = true;

            return fetch("http://127.0.0.1:5000/transform", {
              method: "POST",
              headers: {
                Origin: "" // set our origin here, so only this app is allowed
              },
              body: fd
            })
              .then(response => response.json())
              .then(jsonData => {
                console.log(jsonData);
                let blobData = jsonData.file.blob();
                // replace images at index 0 with new image
                this.$set(this.images, index, {
                  fileurl: URL.createObjectURL(blobData),
                  file: blobData,
                  name: "Processed: " + elem.name,
                  processed: true,
                  loading: false
                });
              })
              .catch(error => console.log(error));
          }
        })
      );
    }
  }
};
</script>

<style lang="scss" scoped>
img {
  object-fit: cover;
}

.image-card {
  .delete-button {
    transition: 0.1s ease-in-out;
    left: 1rem;
    top: 1rem;
    position: absolute;
    z-index: 999;
    opacity: 0;
  }

  &:hover {
    .delete-button {
      opacity: 1;
    }
  }
}
</style>
