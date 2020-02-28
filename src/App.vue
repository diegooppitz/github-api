<!-- <template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>


<script>
import HelloWorld from "./components/HelloWorld.vue";
-->
<template>
  <div id="app">
    <input
      placeholder="Digite nome do usuário"
      type="text"
      v-model="userName"
    />
    <button @click="getRepos()">Buscar</button>

    <template v-if="verif">
      <ul>
        <li>
          <h2>Login {{ login }}</h2>
        </li>
        <li>
          <p>Id {{ id }}</p>
        </li>
        <li>
          <img :src="avatar" alt />
        </li>
      </ul>
    </template>

    <button v-if="verif && !starredRepos" @click="favoritesRepos()">
      Ver repositórios favoritos
    </button>
    <button v-if="verif && starredRepos" @click="getRepos()">
      Ver repositórios públicos
    </button>

    <div v-if="!starredRepos && verif">
      <h2>Repositórios públicos</h2>
      <template class="currency" v-for="repos in reposRequestV.data">
        <div :key="repos.name">
          <a :href="`https://github.com/${userName + '/' + repos.name}`">
            <h3>{{ repos.name }}</h3>
          </a>
        </div>
      </template>
    </div>

    <div v-if="starredRepos">
      <h2>Repositórios favoritos</h2>
      <template v-for="starred in starredRequestV.data">
        <div :key="starred.name">
          <a :href="`https://github.com/${userName + '/' + starred.name}`">
            <h3>{{ starred.name }}</h3>
          </a>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      info: "",
      userName: "",
      reposRequestV: {},
      starredRequestV: {},
      login: "",
      id: "",
      avatar: "",
      verif: false,
      starredRepos: false
    };
  },
  methods: {
    getRepos() {
      axios
        .all([this.userRequest(), this.reposRequest(), this.starredRequest()])
        .then(
          axios.spread((userResponse, reposResponse, starredResponse) => {
            this.login = userResponse.data.login;
            this.id = userResponse.data.id;
            this.avatar = userResponse.data.avatar_url;
            this.reposRequestV = reposResponse;
            this.starredRequestV = starredResponse;
          })
        );

      this.verif = true;
      this.starredRepos = false;
    },
    userRequest() {
      return axios.get("https://api.github.com/users/" + this.userName);
    },
    reposRequest() {
      return axios.get(
        "https://api.github.com/users/" + this.userName + "/repos"
      );
    },
    starredRequest() {
      return axios.get(
        "https://api.github.com/users/" + this.userName + "/starred"
      );
    },
    favoritesRepos() {
      return (this.starredRepos = true);
    }
  }
};
</script>