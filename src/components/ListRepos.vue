<template>
  <div class="listRepos">
    <button class="reposButton" v-if="!starRep" @click="updateRepos()">
      Ver repositórios favoritos
    </button>
    <button class="reposButton" v-if="starRep" @click="updateRepos()">
      Ver repositórios públicos
    </button>

    <div class="repos" v-if="!starRep">
      <h3 class="reposTitle">Repositórios públicos:</h3>
      <template class="currency" v-for="repos in repReqV.data">
        <div :key="repos.name">
          <a
            target="_blank"
            :href="`https://github.com/${userN + '/' + repos.name}`"
          >
            <h4>{{ repos.name }}</h4>
          </a>
          <p class="technologies">{{ repos.language }}</p>
          <p class="date">
            {{ repos.created_at | moment("calendar", "July 10 2011") }}
          </p>
          <br />
          <p class="description">{{ repos.description }}</p>
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
          <p class="technologies">
            {{ starred.language }}
          </p>
          <p class="date">
            {{ starred.created_at | moment("calendar", "July 10 2011") }}
          </p>
          <br />
          <p class="description">{{ starred.description }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
const moment = require("moment");
require("moment/locale/pt-br");

Vue.use(require("vue-moment"), {
  moment
});

export default {
  name: "ListRepos",
  data() {
    return {
      starRep: null,
      technoColor: ""
    };
  },
  props: {
    userN: {
      type: String,
      default: ""
    },
    colab: {
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
    },
    verifTechno() {
      if( this.starred.name == "javaScript")
      this.technoColor = "red";
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
  margin: 0 10px 0 0;
}

.repos h4 {
  margin: 0;
  font-size: 18px;

  &:hover {
    opacity: 0.7;
  }
}

.repos p {
  font-size: 14px;
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

.description {
  margin: 5px 0 20px 0;
}

.technologies {
  margin: 0 10px 0 0;
}

.date {
  margin: 0;
}
</style>
