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

    <pulse-loader v-show="loading"></pulse-loader>
    <template v-if="!loading && valid && verif">
      <User :log="login" v-model="userName" :avat="avatar" :idUser="id"></User>
      <ListRepos
        :userN="userName"
        :repReqV="reposRequestV"
        :starReqV="starredRequestV"
      >
      </ListRepos>
    </template>
    <div v-show="!valid" class="msgError">{{ msgError }}</div>
  </div>
</template>

<script>
import axios from "axios";
import PulseLoader from "@/components/PulseLoader.vue";
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
      this.msgError = "";

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
