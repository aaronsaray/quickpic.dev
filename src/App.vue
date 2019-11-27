<template>
  <div id="app">
    <header class="container-fluid" :class="{ 'has-image': sourceImageObject }">
      <img src="./assets/logo.png" alt="QuickPic" />
      <a
        v-if="sourceImageObject"
        href="#"
        class="btn btn-danger btn-sm"
        id="start-over-button"
        @click.prevent="confirmStartOver()"
      >Start Over</a>
    </header>
    <main class="container-fluid">
      <div
        class="d-md-none alert alert-warning text-center"
      >For optimimal performance, please use this in a larger size browser.</div>
      <div class="row">
        <div v-if="!sourceImageObject" class="col-6">
          <initial-instructions></initial-instructions>
        </div>
        <div v-if="!sourceImageObject" class="col-6">
          <image-loader :image.sync="sourceImageObject"></image-loader>
        </div>
        <div v-if="sourceImageObject" class="col-9">
          <main-image :image="sourceImageObject" @scaled="scale = $event"></main-image>
        </div>
        <div v-if="sourceImageObject" class="col-3">
          <edit-tools :scale="scale"></edit-tools>
        </div>
      </div>
    </main>
    <footer>
      <div class="container-fluid">
        <div class="row align-items-center">
          <div class="col">
            <a href="https://thedevmanager.com" target="_blank">
              <img src="./assets/supportthedevmanager.png" alt="Support by The Dev Manager" />
            </a>
          </div>
          <div class="col vertical-align-middle">
            &copy; 2019
            <a
              href="https://morebetterfaster.io"
              target="_blank"
              title="When you need your product developed more, better, faster..."
            >More Better Faster</a>
          </div>
          <div class="col">
            <a href="https://should-i-fire-my-developer.com" target="_blank">
              <img src="./assets/supportdeveloper.png" alt="Support by MBF" />
            </a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import InitialInstructions from "./components/InitialInstructions";
import ImageLoader from "./components/ImageLoader";
import MainImage from "./components/MainImage";
import EditTools from "./components/EditTools";

export default {
  components: {
    ImageLoader,
    InitialInstructions,
    MainImage,
    EditTools
  },
  data: function() {
    return {
      sourceImageObject: null,
      scale: 1
    };
  },
  methods: {
    confirmStartOver() {
      if (
        confirm(
          "Are you sure you want to start over? Any work you've done will be lost."
        )
      ) {
        this.sourceImageObject = null;
      }
    }
  }
};
</script>

<style lang="scss">
@import "~bootstrap/scss/functions";
@import "~bootstrap/scss/variables";
@import "~bootstrap/scss/mixins";

$body-bg: #f3f3f3;
$custom-file-button-bg: $primary;
$custom-file-button-color: $white;

@import "~bootstrap/scss/bootstrap";

#app {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
main {
  flex-grow: 1;
}

header {
  padding: 16px 0;
  transition: padding 300ms;
  img {
    height: 80px;
    margin-left: calc(50vw - 40px);
    transition: all 500ms;
  }
}
header.has-image {
  padding-top: 0;
  padding-bottom: 2px;
  img {
    margin-left: 0;
    height: 48px;
  }
}

footer {
  margin-top: 1rem;
  background-color: #2e363e;
  color: #aaaaaa;
  padding: 8px 0;
  text-align: center;
  font-size: 10px;
  a,
  a:hover {
    color: #aaaaaa;
  }
}

#start-over-button {
  position: absolute;
  top: 5px;
  right: 8px;
}
</style>
