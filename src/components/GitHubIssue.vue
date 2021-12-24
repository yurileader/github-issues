<template>
  <div class="container">
    <div v-if="loader">
      <img
        class="position-absolute bottom-10 end-50"
        src="../assets/Spinner-animado.svg"
        alt="loading spinner"
      />
    </div>

    <div class="card">
      <div class="card-header text-center">
        <h2>{{ issue.title }}</h2>
      </div>
      <div class="card-body" v-if="!loader && issue.number">
        <h1>Issue #{{ issue.number }}</h1>

        <div class="mt-3">
          {{ issue.body }}
        </div>

        <div class="mt-2">
          <a @click="$router.back()" class="btn btn-success btn-sm">
            Voltar
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "GitHubIssue",

  created() {
    this.buscarIssue();
  },

  data() {
    return {
      issue: {},
      loader: false
    };
  },

  methods: {
    buscarIssue() {
      this.loader = true;
      const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`;
      axios
        .get(url)
        .then(response => {
          this.issue = response.data;
        })
        .finally(() => {
          this.loader = false;
        });
    }
  }
};
</script>

<style></style>
