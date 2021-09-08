<template>
  <div>
    <div class="overlay" v-show="loading">
      <b-spinner class="spinner" variant="primary"></b-spinner>
    </div>
    <div class="container fixed-width">
      <h1>URL shortening service</h1>
      <p>(like [bit.ly](https://bit.ly))</p>
      <b-form-input v-model="url" placeholder="Enter URL"></b-form-input>
      <p v-show="shortenedUrl.length > 0" class="remarks margin-top-10">
        Your shortened url is: {{ shortenedUrl }}
      </p>
      <b-button variant="outline-primary" class="margin-top-10" @click="submit"
        >Shorten</b-button
      >
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { backendUrl } from "../config";

export default {
  name: "HelloWorld",
  data() {
    return {
      url: "",
      shortenedUrl: "",
      loading: false,
    };
  },
  methods: {
    submit() {
      if (this.url.trim().length <= 0) {
        alert("Please enter a URL");
        return;
      }
      if (
        this.url.substring(0, 7) == "http://" ||
        this.url.substring(0, 8) == "https://"
      ) {
        this.loading = true;
        axios
          .post(backendUrl, {
            url: this.url,
          })
          .then((res) => {
            this.shortenedUrl = backendUrl + res.data;
            this.loading = false;
          })
          .catch(() => {
            this.loading = false;
          });
        return;
      }
      alert("Please enter a valid URL starting with http:// or https://");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.fixed-width {
  max-width: 500px;
}
.margin-top-10 {
  margin-top: 10px;
}
.spinner {
  display: block;
  margin: 20px auto;
}
.overlay {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.6);
  padding-top: 100px;
}
</style>
