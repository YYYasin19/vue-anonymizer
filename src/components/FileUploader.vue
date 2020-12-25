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
  </div>
</template>

<script>
export default {
  data() {
    return {
      dropFiles: []
    };
  }
};
</script>

<style lang="scss" scoped></style>
