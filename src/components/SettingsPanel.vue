<template>
  <div class="box">
    <div class="columns">
      <div class="column">
        <b-field
          label="Detection Threshold"
          class="is-primary"
          style="width: 50%"
        >
          <b-slider
            :min="0"
            :max="100"
            :custom-formatter="val => String(val / 100)"
            :step="5"
            lazy
            type="is-primary"
            ticks="true"
            size="is-small"
            v-model="detection_threshold"
          ></b-slider>
          <hr />
          <div class="text">
            Here you can set a detection Threshold
          </div>
        </b-field>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SettingsPanel",
  data() {
    return {
      detection_threshold: 1.0
    };
  }
};
</script>

<style></style>
