<template>
  <div class="listRepos">
    <button class="reposButton" v-if="!starRep" @click="updateRepos()">Ver repositórios favoritos</button>
    <button class="reposButton" v-if="starRep" @click="updateRepos()">Ver repositórios públicos</button>

    <div class="repos" v-if="!starRep">
      <h3 class="reposTitle">Repositórios públicos:</h3>
      <template class="currency" v-for="repos in repReqV.data">
        <div :key="repos.name">
          <a target="_blank" :href="`https://github.com/${userN + '/' + repos.name}`">
            <h4>{{ repos.name }}</h4>
          </a>
          <p>{{ repos.description }}</p>
        </div>
      </template>
    </div>

    <div class="repos" v-if="starRep">
      <h3 class="reposTitle">Repositórios favoritos:</h3>
      <template v-for="starred in starReqV.data">
        <div :key="starred.name">
          <a :href="`https://github.com/${userN + '/' + starred.name}`">
            <h4>{{ starred.name }}</h4>
          </a>
          <p>{{ starred.description }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "ListRepos",
  data() {
    return {
      starRep: null
    };
  },
  props: {
    userN: {
      type: String,
      default: ""
    },
    repReqV: {
      type: Object,
      default: () => {}
    },
    starReqV: {
      type: Object,
      default: () => {}
    }
  },
  methods: {
    updateRepos() {
      this.starRep = !this.starRep;
    }
  }
};
</script>

<style scoped lang="scss">
.reposTitle {
  margin: 40px 0 20px 0;
}

.repos {
  text-align: left;
  width: 80vw;
}

.repos a,
p {
  display: inline-block;
}

.repos a {
  margin: 0 20px 0 0;
}

.repos h4 {
  margin: 10px 0;
  font-size: 20px;

  &:hover {
    opacity: 0.7;
  }
}

.repos p {
  margin: 0 0 15px 0;
  font-size: 16px;
}

.reposButton {
  font-weight: 900;
  border: none;
  background-color: #0000ff;
  color: #ffffff;
  opacity: 0.7;
  margin-top: 25px;
  font-size: 16px;
  width: 250px;
  height: 45px;
  cursor: pointer;

  &:hover {
    opacity: 0.5;
  }
}
</style>
