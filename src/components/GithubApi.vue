<template>
  <div class="github-api">
    <h1>Api do Github</h1>
    <p class="searchText">Busque dados de users do Github</p>
    <input
      placeholder="Digite um username do Github"
      type="text"
      v-model="userName"
    />
    <button class="searchButton" @click="getRepos()">Buscar</button>

    <pulse-loader v-if="loading"></pulse-loader>
    <template v-if="!loading && valid && verif">
      <User :login="login" v-model="userName" :avatar="avatar" :id="id"></User>
      <ListRepos
        :userName="userName"
        :starredRepos="starredRepos"
        :reposRequestV="reposRequestV"
        :starredRequestV="starredRequestV"
        @update="starredRepos = $event"
      >
      </ListRepos>
      <div class="msgError">{{ msgError }}</div>
    </template>
  </div>
</template>

<script>
// import Vue from 'vue'
import axios from "axios";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import User from "@/components/User";
import ListRepos from "@/components/ListRepos";

export default {
  name: "GithubApi",
  components: {
    PulseLoader,
    User,
    ListRepos
  },
  data() {
    return {
      info: "",
      userName: "",
      reposRequestV: {},
      starredRequestV: {},
      login: "",
      id: "",
      avatar: "",
      starredRepos: false,
      loading: false,
      valid: true,
      verif: false,
      msgError: ""
    };
  },
  methods: {
    getRepos() {
      this.loading = true;
      this.valid = true;
      this.verif = true;
      this.starredRepos = false;

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
        )
        .catch(error => {
          this.valid = false;
          if (error.response.status == 404) {
            return (this.msgError = "Este username é inexistente");
          } else if (error.response.status == 403) {
            return (this.msgError =
              "Você atingiu o limite de requisições da Api do Github");
          }
        })
        .finally(() => (this.loading = false));
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
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
// Geral
.github-api {
  position: absolute;
  text-align: center;
  transform: translate(-50%, 0);
  left: 50%;
  top: 10%;
  margin-bottom: 10%;
}

// Search
h1 {
  margin-bottom: 5px;
}

.searchText {
  margin-bottom: 40px;
}

input {
  width: 260px;
  height: 40px;
  font-size: 16px;
  padding-left: 15px;
}

input:focus {
  outline: none;
}

.msgError {
  font-size: 24px;
  position: relative;
  top: 40px;
}

// Search Button
input,
.searchButton {
  display: inline-block;
  border-radius: 2px;
}

.searchButton {
  font-weight: 900;
  border: none;
  background-color: #0000ff;
  color: #ffffff;
  opacity: 0.7;
  cursor: pointer;

  &:hover {
    opacity: 0.5;
  }
}

.searchButton {
  width: 130px;
  height: 45px;
  font-size: 16px;
  padding-top: 2px;

  @media (max-width: 768px) {
    margin-top: 20px;
    width: 200px;
  }
}
</style>
