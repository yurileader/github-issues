<template>
  <div class="container">
    <form>
      <div class="row">
        <div class="col-5">
          <input
            v-model="username"
            type="text"
            placeholder="GitHub usarname"
            class="form-control"
            id="gitUsername"
          />
        </div>
        <div class="col-5">
          <input
            v-model="repositorio"
            type="text"
            placeholder="GitHub repositório"
            class="form-control"
            id="gitRepositorio"
          />
        </div>
        <div class="col-2 d-flex">
          <button
            @click.prevent.stop="buscarIssues()"
            type="submit"
            class="btn btn-success btn-sm"
          >
            BUSCAR
          </button>
          <button
            @click.prevent.stop="limpar()"
            type="button"
            class="btn btn-danger btn-sm ms-1"
          >
            LIMPAR
          </button>
        </div>
      </div>
    </form>

    <div class="mt-3">
      <table class="table table-striped table-hover">
        <thead>
          <tr>
            <th scope="col">Número</th>
            <th scope="col">Título</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="loader">
            <td colspan="2" class="text-center">
              <img
                class="position-absolute top-50 bottom-10 end-50"
                src="../assets/Spinner-animado.svg"
                alt="loading spinner"
              />
            </td>
          </tr>
          <tr v-for="issue in issueslist" :key="issue.id">
            <td>
              <router-link
                :to="{
                  name: 'GitHubIssue',
                  params: {
                    name: username,
                    repo: repositorio,
                    issue: issue.number
                  }
                }"
              >
                {{ issue.number }}
              </router-link>
            </td>
            <td>{{ issue.title }}</td>
          </tr>
          <tr v-if="!issueslist.length">
            <td>#</td>
            <td>Nenhuma issues encontrada!</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "GitHubIssues",

  data() {
    return {
      username: "",
      repositorio: "",
      loader: false,
      issueslist: []
    };
  },
  created() {
    this.getLocalStorage();
  },

  methods: {
    buscarIssues() {
      if (this.username && this.repositorio) {
        localStorage.setItem("gitHubIssues",
          JSON.stringify({username: this.username,repository: this.repositorio})
        );
        this.loader = true;
        const url = `https://api.github.com/repos/${this.username}/${this.repositorio}/issues`;
        axios
          .get(url)
          .then(response => {
            this.issueslist = [...response.data];
          })
          .finally(() => {
            this.loader = false;
          });
      }
    },

    limpar() {
      (this.username = ""), (this.repositorio = ""), (this.issueslist = []);
    },

    getLocalStorage() {
      const localData = JSON.parse(localStorage.getItem("gitHubIssues"));

      if (localData.username && localData.repository) {
        this.username = localData.username;
        this.repositorio = localData.repository;
        this.buscarIssues();
      }
    }
  }
};
</script>

<style></style>
