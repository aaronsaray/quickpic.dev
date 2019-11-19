<template>
  <div>
    <div
      v-if="!imageSource"
      id="uploader"
      :class="{ hovering: hovering }"
      ref="uploader"
    >
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
            accept="image/*"
            @change="loadImageFromFile"
          />
          <label class="custom-file-label" for="upload-file">Choose file</label>
        </div>
      </div>
      <div class="text-center mb-3 text-muted">
        - or -
      </div>
      <div class="text-center paste-message mb-3">
        Paste Image From Clipboard
      </div>
      <div class="text-center mb-3 text-muted">
        - or -
      </div>
      <div class="text-center" :class="{ 'font-weight-bold': hovering }">
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
      loadingUrl: false,
      hovering: false
    };
  },

  mounted() {
    this.initDragAndDrop();
    this.initPaste();
  },

  methods: {
    /**
     * This will load an image into the data from a drag and drop
     */
    loadImageFromDrop(event) {
      if (this.imageSource) {
        return;
      }

      this.loadImageFromFileElement(event.dataTransfer.files[0]);
    },

    /**
     * This processes the image from the file upload
     */
    loadImageFromFile(event) {
      this.loadImageFromFileElement(event.target.files[0]);
    },

    /**
     * This is shared between drag and drop and file - basically processes it as a data url
     */
    loadImageFromFileElement(file) {
      const reader = new FileReader();
      reader.onload = e => {
        this.imageSource = e.target.result;
        this.emitHasImage();
      };
      reader.readAsDataURL(file);
    },

    /**
     * Tries to load the image from a url - loads it into an image first to make sure it can load, then swaps it into the dom
     */
    loadImageFromUrl() {
      this.loadingUrl = true;
      let image = new Image();
      let vue = this;

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

    /**
     * processes loading this from a paste
     */
    loadImageFromPaste(event) {
      if (this.imageSource) {
        return;
      }

      const itemPaste = [...event.clipboardData.items].find(
        i => i.type.indexOf("image") !== -1
      );
      if (itemPaste) {
        const image = window.URL.createObjectURL(itemPaste.getAsFile());
        this.imageSource = image;
        this.emitHasImage();
      } else {
        alert(
          "Could not load an image from your clipboard.  Please make sure it's an image, and not a copy of a file."
        );
      }
    },

    /**
     * How we tell the parent we're ready to do the damn thang
     */
    emitHasImage() {
      this.$emit("has-image", true);
    },

    /**
     * When someone is dragging an image over
     */
    doHovering() {
      this.hovering = true;
    },

    /**
     * When they stop dragging an image over
     */
    stopHovering() {
      this.hovering = false;
    },

    /**
     * This initializes the drag and drop functionality for the images
     */
    initDragAndDrop() {
      let uploader = this.$refs.uploader;
      uploader.addEventListener("dragenter", this.doHovering, false);
      uploader.addEventListener("dragover", this.doHovering, false);
      uploader.addEventListener("dragleave", this.stopHovering, false);
      uploader.addEventListener("drop", this.stopHovering, false);
      ["dragenter", "dragover", "dragleave", "drop"].forEach(eventName => {
        uploader.addEventListener(
          eventName,
          e => {
            e.preventDefault();
            e.stopPropagation();
          },
          false
        );
      });
      uploader.addEventListener("drop", this.loadImageFromDrop, false);
    },

    /**
     * Handles the paste of an image
     */
    initPaste() {
      window.addEventListener("paste", this.loadImageFromPaste);
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
  border: 2px dashed transparent;
  &.hovering {
    border-color: rgba(0, 0, 0, 0.5);
    .input-group,
    .text-muted,
    .paste-message {
      opacity: 0.3;
    }
  }
}
#main-image {
  img {
    display: block;
    margin: auto;
    max-width: 100%;
    height: auto;
    padding: 10px;

    background-color: #fefefe;
    background-image: linear-gradient(
        45deg,
        #cbcbcb 25%,
        transparent 25%,
        transparent 75%,
        #cbcbcb 75%,
        #cbcbcb
      ),
      linear-gradient(
        45deg,
        #cbcbcb 25%,
        transparent 25%,
        transparent 75%,
        #cbcbcb 75%,
        #cbcbcb
      );
    -webkit-background-size: 30px 30px;
    -moz-background-size: 30px 30px;
    background-size: 30px 30px;
    background-position: 0 0, 15px 15px;
  }
}
</style>
