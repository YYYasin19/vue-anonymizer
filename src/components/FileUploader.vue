<template>
  <div>
    <b-field>
      <b-upload v-model="dropFiles" multiple drag-drop>
        <section class="section">
          <div class="content has-text-centered">
            <p>
              <b-icon icon="upload" size="is-large"> </b-icon>
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
    }
  }
};
</script>

<style lang="scss" scoped>
.uploaded-images {
  object-fit: cover;
}

.image-area {
  transition: 1s ease-in-out;
  &:hover {
    .image-tag {
      display: block;
    }
  }
}
.image-tag {
  display: none;
}
</style>
