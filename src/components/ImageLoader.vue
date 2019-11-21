<template>
  <div>
    <div id="uploader" :class="{ hovering: hovering }" ref="uploader">
      <h3 class="text-center mb-3">Pick Your Image</h3>
      <form @submit.prevent="loadImageFromUrl()">
        <div class="input-group mb-3">
          <input
            type="url"
            class="form-control"
            placeholder="Enter full URL"
            v-model="url"
            @focus="textInput = true"
            @blur="textInput = false"
          />
          <div class="input-group-append">
            <button class="btn btn-primary" :class="{ disabled: loadingUrl }" type="submit">
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
      <div class="text-center mb-3 text-muted">- or -</div>
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
      <div class="text-center mb-3 text-muted">- or -</div>
      <div class="text-center paste-message mb-3">Paste Image From Clipboard</div>
      <div class="text-center mb-3 text-muted">- or -</div>
      <div class="text-center" :class="{ 'font-weight-bold': hovering }">Drop Your Image Here</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: Object
  },

  data: function() {
    return {
      url: null,
      loadingUrl: false,
      hovering: false,
      textInput: false
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
        let image = new Image();
        image.onload = () => {
          this.displayLoadedImage(image);
        };
        image.src = e.target.result;
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
        vue.displayLoadedImage(this);
        vue.loadingUrl = false;
      };

      image.onerror = function() {
        alert(
          "That image couldn't be loaded. Please check the URL and try again."
        );
        vue.loadingUrl = false;
      };

      image.src = this.url;
    },

    /**
     * processes loading this from a paste
     */
    loadImageFromPaste(event) {
      if (this.textInput) {
        return;
      }

      const itemPaste = [...event.clipboardData.items].find(
        i => i.type.indexOf("image") !== -1
      );
      if (itemPaste) {
        let image = new Image();
        image.onload = () => {
          this.displayLoadedImage(image);
        };
        image.src = window.URL.createObjectURL(itemPaste.getAsFile());
      } else {
        alert(
          "Could not load an image from your clipboard.  Please make sure it's an image, and not a copy of a file."
        );
      }
    },

    /**
     * Draws image to the canvas
     */
    displayLoadedImage(imageElement) {
      this.$emit("update:image", imageElement);
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
</style>
