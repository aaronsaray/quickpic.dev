<template>
  <div>
    <div v-if="!image" id="uploader">
      <h3 class="text-center mb-3">Pick Your Image</h3>
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Enter full URL" />
        <div class="input-group-append">
          <button class="btn btn-primary" type="button">
            Go
          </button>
        </div>
      </div>
      <div class="text-center mb-3 text-muted">
        - or -
      </div>
      <div class="input-group mb-3">
        <div class="custom-file">
          <input
            type="file"
            class="custom-file-input"
            id="upload-file"
            @change="loadImageFromFile"
          />
          <label class="custom-file-label" for="upload-file">Choose file</label>
        </div>
      </div>
      <div class="text-center mb-3 text-muted">
        - or -
      </div>
      <div class="text-center">
        Drop Your Image Here
      </div>
    </div>
    <div v-else id="main-image">
      <img :src="image" />
    </div>
  </div>
</template>

<script>
export default {
  name: "MainImage",
  props: {
    msg: String
  },
  data: function() {
    return {
      image: null
    };
  },

  methods: {
    loadImageFromFile(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = e => {
        this.image = e.target.result;
        this.emitHasImage();
      };
      reader.readAsDataURL(file);
    },

    emitHasImage() {
      this.$emit("has-image", true);
    }
  }
};
</script>

<style lang="scss">
#uploader {
  background: #f9f9f9;
  margin: 1rem;
  padding: 1rem;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}
#main-image {
  background: #aaaaaa;
  padding: 1rem;
  img {
    max-width: 100%;
    height: auto;
  }
}
</style>
