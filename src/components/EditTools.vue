<template>
  <div>
    <div class="card mb-2">
      <h5 class="text-center card-header">Details</h5>
      <div class="card-body">
        <strong>Zoom:</strong>
        {{ this.zoomPercentage }}%
      </div>
    </div>
    <hr />
    <div class="card">
      <div class="card-body">
        <form @submit.prevent="handleDownloadRequest()">
          <div class="input-group">
            <input
              type="text"
              class="form-control form-control-sm"
              placeholder="filename"
              required
              v-model="filename"
            />
            <div class="input-group-append">
              <button class="btn btn-primary btn-sm" type="submit">Download</button>
            </div>
          </div>
          <small class="form-text text-muted">
            This will download as a
            <code>.png</code> file
          </small>
        </form>
        <div class="text-muted text-center small my-2">- or -</div>
        <div class="text-center">
          <a
            href="#"
            class="btn btn-link btn-sm"
            @click.prevent="handleOpenInNewTab()"
          >Open in New Tab</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EventBus from "../EventBus";

export default {
  props: {
    scale: [Number, String]
  },

  data: function() {
    return {
      filename: "quickpic.png"
    };
  },

  methods: {
    /**
     * Append PNG if we don't have one, then tell the parent to download it
     */
    handleDownloadRequest() {
      let extension = this.filename.split(".").pop();
      if (extension !== "png") {
        this.filename += ".png";
      }

      EventBus.$emit("edit-tools:download", this.filename);
    },

    /**
     * Issues the event that says open this image in a new tab
     */
    handleOpenInNewTab() {
      EventBus.$emit("edit-tools:open-in-new-tab");
    }
  },

  computed: {
    zoomPercentage() {
      return this.scale * 100;
    }
  }
};
</script>