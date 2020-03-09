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
          <p class="technologies" :class="verifTechno(repos.language)">{{ repos.language }}</p>
          <p class="date">{{ repos.created_at | moment("calendar", "July 10 2011") }}</p>
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
          <p class="technologies" :class="verifTechno(starred.language)">{{ starred.language }}</p>
          <p class="date">{{ starred.created_at | moment("calendar", "July 10 2011") }}</p>
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
    verifTechno(value) {
      if (value && value.length > 1) {
        switch (value) {
          case "JavaScript":
            return "burntYellow";
            break;
          case "TypeScript":
            return "typeScriptColor";
            break;
          case "React":
            return "coral";
            break;
          case "Ruby":
            return "DarkGoldenRod";
            break;
          case "Java":
            return "dimGrey";
            break;
          case "PHP":
            return "phpColor";
            break;
          case "HTML":
            return "htmlColor";
            break;
          case "Python":
            return "green";
            break;
          case "CSS":
            return "red";
            break;
          case "Shell":
            return "blue";
            break;
          case "Go":
            return "yellow";
            break;
          case "Vue":
            return "vueColor";
            break;
          default:
            return "black";
            break;
        }
      } else {
        return "notTechno";
      }
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
  font-size: 22px;

  &:hover {
    opacity: 0.7;
  }
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
  font-size: 18px;
}

.date {
  margin: 0;
}

.technologies {
  margin: 0 10px 0 0;
  padding: 3px 12px;
  font-size: 10px;
  font-weight: 900;
  color: #ffffff;
}

.blue {
  background-color: #0000ff;
}

.burntYellow {
  background-color: #e1ad01;
}

// Labels Colors
.red {
  background-color: #dc143c;
}

.black {
  background-color: #000;
}

.coral {
  background-color: #FF7F50
}

.DarkGoldenRod {
  background-color: #B8860B;
}

.yellow {
  background-color: #ffff00;
}

.dimGrey {
  background-color: #696969;
}

.green {
  background-color: green;
}

.orange {
  background-color: #ff8c00;
}

.htmlColor {
  background-color: #e75f35;
}

.phpColor {
  background-color: #5283a3;
}

.typeScriptColor {
  background-color: #715cc1;
}

.vueColor {
  background-color: #42b884;
}

.notTechno {
  margin: initial;
  padding: initial;
  font-size: initial;
  font-weight: initial;
  color: initial;
}
</style>
