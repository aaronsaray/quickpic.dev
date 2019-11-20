<template>
  <div id="main-image" ref="main-image">
    <div class="matte">
      <canvas ref="canvas" />
    </div>
  </div>
</template>

<script>
import EventBus from "../EventBus";

export default {
  props: {
    image: HTMLImageElement
  },

  created() {
    EventBus.$on("edit-tools:download", filename =>
      this.downloadImage(filename)
    );
  },

  mounted() {
    this.displayLoadedImage();
  },

  methods: {
    /**
     * Draws image to the canvas
     */
    displayLoadedImage() {
      const canvas = this.$refs.canvas;
      const imageContainer = this.$refs["main-image"];

      let imageWidth = this.image.naturalWidth;
      let imageHeight = this.image.naturalHeight;
      let containerWidth = imageContainer.clientWidth;

      let canvasWidth = imageWidth;
      let canvasHeight = imageHeight;

      if (imageWidth > containerWidth) {
        let scale = (containerWidth / imageWidth).toFixed(2);
        this.$emit("scaled", scale);
        canvasWidth *= scale;
        canvasHeight *= scale;
      }

      canvas.width = canvasWidth;
      canvas.height = canvasHeight;
      canvas.imageSmoothingEnabled = false;

      let context = canvas.getContext("2d");
      context.drawImage(this.image, 0, 0, canvasWidth, canvasHeight);
    },

    /**
     * Download the edited image
     * @see https://stackoverflow.com/questions/37135417/download-canvas-as-png-in-fabric-js-giving-network-error/
     */
    downloadImage(filename) {
      const canvas = this.$refs.canvas;
      let imageData = canvas.toDataURL("image/png");

      const dataURLtoBlob = dataurl => {
        let arr = dataurl.split(","),
          mime = arr[0].match(/:(.*?);/)[1],
          bstr = atob(arr[1]),
          n = bstr.length,
          u8arr = new Uint8Array(n);
        while (n--) {
          u8arr[n] = bstr.charCodeAt(n);
        }
        return new Blob([u8arr], { type: mime });
      };
      let imageBlob = dataURLtoBlob(imageData);
      let objectUrl = URL.createObjectURL(imageBlob);

      let a = document.createElement("a");
      a.setAttribute("download", filename);
      a.setAttribute("href", objectUrl);
      a.click();
    }
  }
};
</script>

<style lang="scss">
#main-image {
  display: flex;
  justify-content: center;

  .matte {
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
    background-position: -5px -5px, 10px 10px;
  }
}
</style>
