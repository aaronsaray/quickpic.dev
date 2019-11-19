<template>
  <div id="app">
    <header class="container-fluid" :class="{ 'has-image': hasImage }">
      <img src="./assets/logo.png" alt="QuickPic" />
      <a
        v-if="hasImage"
        href="#"
        class="btn btn-danger btn-sm"
        id="start-over-button"
        @click.prevent="confirmStartOver()"
      >Start Over</a>
    </header>
    <main class="container-fluid">
      <div class="row">
        <div v-if="!hasImage" class="col-6">
          <h1>Quickly Edit Your Picture</h1>
          <h2>Edit and touch up your picture just in the browser.</h2>
          <p>
            Make a meme. Resize or crop your image. Apply filters. Download it
            immediately, for free. No need to install any software.
          </p>
          <p>
            Need to quickly create an image for Facebook or Instagram? You
            shouldn't have to download or install software just to quickly edit
            and paste a new image to your favorite sites. Use QuickPic to do a
            few quick adjustments to your image and be on your way.
          </p>
          <p>
            Drag and drop your image in, put in a URL for the image, or upload
            one.
          </p>
        </div>
        <div :class="{ 'col-6': !hasImage, 'col-9': hasImage }">
          <MainImage @has-image="hasImage = true"></MainImage>
        </div>
        <div v-if="hasImage" class="col-3">
          <h3>Tools</h3>
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
import MainImage from "./components/MainImage.vue";

export default {
  name: "app",
  components: {
    MainImage
  },
  data: function() {
    return {
      hasImage: false
    };
  },
  methods: {
    confirmStartOver() {
      if (
        confirm(
          "Are you sure you want to start over? Any work you've done will be lost."
        )
      ) {
        this.hasImage = false;
      }
    }
  }
};
</script>

<style lang="scss">
@import "~bootstrap/scss/functions";
@import "~bootstrap/scss/variables";
@import "~bootstrap/scss/mixins";

$body-bg: #efefef;
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
