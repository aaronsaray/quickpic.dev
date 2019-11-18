<template>
  <div id="app">
    <header class="container-fluid">
      <img src="./assets/logo.png" alt="QuickPic" />
    </header>
    <main class="container-fluid">
      <div class="row">
        <div class="col">
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
        <div class="col px-5">
          <div id="uploader">
            <h3 class="text-center mb-3">Pick Your Image</h3>
            <div class="input-group mb-3">
              <input
                type="text"
                class="form-control"
                placeholder="Enter full URL"
              />
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
                <input type="file" class="custom-file-input" id="upload-file" />
                <label class="custom-file-label" for="upload-file"
                  >Choose file</label
                >
              </div>
            </div>
            <div class="text-center mb-3 text-muted">
              - or -
            </div>
            <div class="text-center">
              Drop Your Image Here
            </div>
          </div>
        </div>
      </div>
    </main>
    <footer>
      <div class="container-fluid">
        <div class="row align-items-center">
          <div class="col">
            <a href="#"><img src="https://placekitten.com/250/50" alt="Ad"/></a>
          </div>
          <div class="col vertical-align-middle">
            &copy; 2019
            <a
              href="https://morebetterfaster.io"
              target="_blank"
              title="When you need your product developed more, better, faster..."
              >More Better Faster</a
            >
          </div>
          <div class="col">
            <a href="#"><img src="https://placekitten.com/250/50" alt="Ad"/></a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "app"
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
  text-align: center;
  padding: 1rem 0;
  img {
    height: 5rem;
  }
}
footer {
  background-color: #2e363e;
  color: #aaaaaa;
  padding: 0.5rem 0;
  text-align: center;
  a,
  a:hover {
    color: #aaaaaa;
  }
}

#uploader {
  background: #f9f9f9;
  padding: 1rem;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}
</style>
