<template>
  <div class="user">
    <div>
      Username:
      <h3>
        <a target="_blank" :href="`https://github.com/${log}`">{{ log }}</a>
      </h3>
      Id:
      <h4>{{ idUser }}</h4>
      <h4 class="company" v-show="comp != null">{{ comp }}</h4>
    </div>
    <img :src="avat" />
  </div>
</template>

<script>
export default {
  name: "User",
  props: {
    log: {
      type: String,
      default: ""
    },
    userN: {
      type: String,
      default: ""
    },
    avat: {
      type: String,
      default: ""
    },
    comp: {
      type: String,
      default: ""
    },
    idUser: {
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

.company {
  margin-left: 40px;
}

.user h3 {
  margin-right: 35px;
}

img {
  width: 300px;
  height: 300px;
  border-radius: 20%;
}
</style>
