<template>
  <div id="main-image" ref="main-image">
    <div class="matte">
      <canvas ref="canvas" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: HTMLImageElement
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
        canvasWidth *= scale;
        canvasHeight *= scale;
      }

      canvas.width = canvasWidth;
      canvas.height = canvasHeight;
      canvas.imageSmoothingEnabled = false;

      let context = canvas.getContext("2d");
      context.drawImage(this.image, 0, 0, canvasWidth, canvasHeight);
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
