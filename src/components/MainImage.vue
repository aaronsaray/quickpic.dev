<template>
  <div id="main-image" ref="main-image">
    <div class="matte" ref="matte">
      <canvas ref="canvas">Canvas support is required to use this website.</canvas>
      <span id="meme-header" v-bind:style="memeStyle">{{ this.meme.header }}</span>
      <span id="meme-footer" v-bind:style="memeStyle">{{ this.meme.footer }}</span>
    </div>
  </div>
</template>

<script>
import EventBus from "../EventBus";

export default {
  props: {
    image: HTMLImageElement
  },

  data: function() {
    return {
      scale: 1,
      meme: {
        header: "",
        footer: ""
      }
    };
  },

  created() {
    EventBus.$on("edit-tools:download", filename =>
      this.downloadImage(filename)
    );

    EventBus.$on("edit-tools:open-in-new-tab", this.openImageInNewTab);

    EventBus.$on("edit-tools:meme", this.applyMeme);
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
        this.scale = (containerWidth / imageWidth).toFixed(2);
        // then scale it to the lowest 5 increment
        this.scale = (Math.floor((this.scale * 100) / 5) * 5) / 100;

        this.$refs.matte.style.transform = `scale(${this.scale})`;
      }

      canvas.width = canvasWidth;
      canvas.height = canvasHeight;
      canvas.imageSmoothingEnabled = false;

      this.$emit("scaled", this.scale);

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
    },

    /**
     * Attempts to open image in new tab
     */
    openImageInNewTab() {
      this.$nextTick(() => {
        let canvas = this.$refs.canvas;
        let imageData = canvas.toDataURL("png");
        let popUp = window.open();
        popUp.document.write('<img src="' + imageData + '"/>');
      });
    },

    /**
     * Apply meme from the tools
     */
    applyMeme(meme) {
      this.meme.header = meme.header.toUpperCase();
      this.meme.footer = meme.footer.toUpperCase();
    }
  },

  computed: {
    memeStyle() {
      let size = 48 / this.scale;
      let padding = 10 / this.scale;
      return {
        fontSize: `${size}px`,
        padding: `${padding}px`
      };
    }
  }
};
</script>

<style lang="scss">
#main-image {
  display: flex;
  justify-content: center;

  .matte {
    position: relative;
    transform-origin: top center;

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
#meme-header,
#meme-footer {
  display: block;
  position: absolute;
  left: 0;
  text-align: center;
  width: 100%;
  color: white;
  text-shadow: -2px -2px 0 #000, 2px -2px 0 #000, -2px 2px 0 #000,
    2px 2px 0 #000;
  font-family: Impact, Charcoal, sans-serif;

  // these will be altered
  padding: 10px;
  font-size: 48px;
}
#meme-header {
  top: 0px;
}
#meme-footer {
  bottom: 0px;
}
</style>
