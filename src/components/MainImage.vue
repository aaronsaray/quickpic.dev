<template>
  <div>
    <div v-if="!imageSource" id="uploader">
      <h3 class="text-center mb-3">Pick Your Image</h3>
      <form @submit.prevent="loadImageFromUrl()">
        <div class="input-group mb-3">
          <input
            type="url"
            class="form-control"
            placeholder="Enter full URL"
            v-model="url"
          />
          <div class="input-group-append">
            <button
              class="btn btn-primary"
              :class="{ disabled: loadingUrl }"
              type="submit"
            >
              Go
              <span
                v-if="loadingUrl"
                class="spinner-border spinner-border-sm"
                role="status"
                aria-hidden="true"
              ></span>
            </button>
          </div>
        </div>
      </form>
      <div class="text-center mb-3 text-muted">
        - or -
      </div>
      <div class="input-group mb-3">
        <div class="custom-file">
          <input
            type="file"
            class="custom-file-input"
            id="upload-file"
            accept="image/gif, image/jpeg, image/png"
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
      <img :src="imageSource" />
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
      imageSource: null,
      url: null,
      loadingUrl: false
    };
  },

  methods: {
    loadImageFromFile(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = e => {
        this.imageSource = e.target.result;
        this.emitHasImage();
      };
      reader.readAsDataURL(file);
    },

    loadImageFromUrl() {
      this.loadingUrl = true;
      let image = new Image();
      let vue = this;

      // make sure it can load
      image.onload = function() {
        vue.imageSource = this.src;
        vue.emitHasImage();
        this.loadingUrl = false;
      };

      image.onerror = function() {
        alert(
          "That image couldn't be loaded. Please check the URL and try again."
        );
        this.loadingUrl = false;
      };

      image.src = this.url;
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
