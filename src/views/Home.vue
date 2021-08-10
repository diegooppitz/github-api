<template>
  <div class="github-api">
    <h1>Github API</h1>
    <p class="searchText">Search Github Users</p>
    <input
      placeholder="Write a Github username"
      type="text"
      v-model="username"
      @keypress.enter="getRepos()"
    />
    <button class="searchButton" @click="getRepos()">Buscar</button>

    <pulse-loader v-show="loading"></pulse-loader>
    <template v-if="!loading && valid && verif">
      <user :log="login" v-model="username" :avat="avatar" :idUser="id" :comp="company"></user>
      <repos-list
        :userN="username"
        :repReqV="reposRequestV"
        :starReqV="starredRequestV"
        :colab="colaborators"
      >
      </repos-list>
    </template>
    <div v-show="!valid" class="msgError">{{ msgError }}</div>
  </div>
</template>

<script>
import axios from "axios";
import PulseLoader from "@/components/PulseLoader.vue";
import User from "@/components/User";
import ReposList from "@/components/ReposList";

export default {
  name: "GithubApi",
  components: {
    PulseLoader,
    User,
    ReposList
  },
  data() {
    return {
      info: "",
      username: "",
      reposRequestV: {},
      starredRequestV: {},
      login: "",
      id: "",
      avatar: "",
      loading: false,
      valid: true,
      verif: false,
      msgError: "",
      colaborators: ""
    };
  },
  methods: {
    getRepos() {
      this.loading = true;
      this.valid = true;
      this.verif = true;
      this.msgError = "";

      axios
        .all([this.userRequest(), this.reposRequest(), this.starredRequest()])
        .then(
          axios.spread((userResponse, reposResponse, starredResponse) => {
            // User
            this.login = userResponse.data.login;
            this.id = userResponse.data.id;
            this.avatar = userResponse.data.avatar_url;
            this.company = userResponse.data.company;

            // Public repos
            this.reposRequestV = reposResponse;

            // Starred Repos
            this.starredRequestV = starredResponse;

            // Test
            this.user = userResponse.data;
            this.numPublicRepos = userResponse.data.public_repos;
          })
        )
        .catch(error => {
          this.valid = false;
          if (error.response.status == 404) {
            return (this.msgError = "Este username não existe");
          } else if (error.response.status == 403) {
            return (this.msgError =
              "Você atingiu o limite de requisições da Api do Github");
          }
        })
        .finally(() => (this.loading = false));

      // console.log(this.user);
    },
    userRequest() {
      return axios.get("https://api.github.com/users/" + this.username);
    },
    reposRequest() {
      return axios.get(
        "https://api.github.com/users/" + this.username + "/repos"
      );
    },
    starredRequest() {
      return axios.get(
        "https://api.github.com/users/" + this.username + "/starred"
      );
    }
  }
};
</script>

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
  outline: none;
  border: solid 1px #ccc;
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
  width: 130px;
  height: 45px;
  font-size: 16px;
  padding-top: 2px;
  cursor: pointer;

  @media (max-width: 768px) {
    margin-top: 20px;
    width: 200px;
  }

  &:hover {
    opacity: 0.5;
  }
}
</style>
