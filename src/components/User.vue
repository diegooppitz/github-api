<template>
  <div class="user">
    <div class="user-child">
      <div class="user-content">
        Username:
        <h3>
          <a target="_blank" :href="`https://github.com/${log}`">{{ log }}</a>
        </h3>
      </div>
      <div class="user-content">
        Id:
        <h4>{{ idUser }}</h4>
      </div>
      <div class="user-content">
        <h4 class="company" v-show="comp != null">{{ comp }}</h4>
      </div>
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
.user-content {
  display: inline-block;

  @media (max-width: 767px) {
    display: block;
  }
}

.user-child {
  @media (max-width: 767px) {
    margin: 15px 0;
  }
}

.user h3,
h4 {
  display: inline-block;

  @media (max-width: 767px) {
    margin: 12px 0;
  }
}

.user h3 {
  margin-right: 35px;

  @media (max-width: 767px) {
    margin: 12px 0;
  }

  &:hover {
    opacity: 0.5;
  }
}

.company {
  margin-left: 40px;

  @media (max-width: 767px) {
    margin-left: initial;
  }
}

img {
  width: 300px;
  height: 300px;
  border-radius: 10px;
}
</style>
