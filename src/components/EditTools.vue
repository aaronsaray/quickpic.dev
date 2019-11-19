<template>
  <div>
    <h3>Tools</h3>
    <div class="card">
      <div class="card-body">
        <form @submit.prevent="handleDownloadRequest()">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="filename"
              required
              v-model="filename"
            />
            <div class="input-group-append">
              <button class="btn btn-primary" type="submit">Download</button>
            </div>
          </div>
          <small class="form-text text-muted">
            This will download as a
            <code>.png</code> file
          </small>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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

      this.$emit("download", this.filename);
    }
  }
};
</script>