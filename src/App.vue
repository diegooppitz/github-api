<template>
  <div id="app">
    <GithubApi></GithubApi>
  </div>
</template>

<script>
import GithubApi from "./components/GithubApi.vue";

export default {
  name: "App",
  components: {
    GithubApi
  }
};
</script>
