<template>
  <div class="user">
    <div>
      Username:
      <h3>
        <a target="_blank" :href="`https://github.com/${userName}`">{{ login }}</a>
      </h3>
      <h4>{{ id }}</h4>
    </div>
    <img :src="avatar" />
  </div>
</template>

<script>
export default {
  name: "User",
  props: {
    login: {
      type: String,
      default: ""
    },
    userName: {
      type: String,
      default: ""
    },
    avatar: {
      type: String,
      default: ""
    },
    id: {
      type: Number,
      default: null
    }
  }
};
</script>

<style scoped lang="scss">
.user h3,
h4 {
  display: inline-block;
}

.user h3 {
  margin-right: 35px;
}

img {
  width: 300px;
  height: 300px;
}
</style>
