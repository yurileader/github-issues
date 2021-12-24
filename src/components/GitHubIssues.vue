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

    <template v-if="selectedIssue.id">
      <h2 class="mt-3">{{ selectedIssue.title }}</h2>
      {{ selectedIssue.body }}
      <div class="position-absolute bottom">
        <button
          @click.prevent.stop="voltar()"
          type="submit"
          class="btn btn-primary btn-sm"
        >
          Voltar
        </button>
      </div>
    </template>

    <div class="mt-3" v-if="!selectedIssue.id">
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
              <div class="spinner-border text-primary" role="status">
                <span class="visually-hidden">Loading...</span>
              </div>
            </td>
          </tr>
          <tr v-for="issue in issueslist" :key="issue.id">
            <td>
              <a
                @click.prevent.stop="buscarIssueId(issue)"
                class="text-decoration-none fw-bold"
                href=""
                >{{ issue.number }}</a
              >
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
import Vue from 'vue'

export default {
  name: "GitHubIssues",

  data() {
    return {
      username: "",
      repositorio: "",
      selectedIssue: {},
      loader: false,
      userlist: []
    };
  },

  methods: {
    buscarIssues() {
      if (this.username && this.repositorio) {
        this.loader = true;
        const url = `https://api.github.com/repos/${this.username}/${this.repositorio}/issues`;
        axios
          .get(url)
          .then(response => {
            this.issueslist = [...response.data];
          })
          .finally(() => {
            this.loader = false;
          })
          .catch(erro => {
            this.issueslist = [];
          });
      }
    },

    buscarIssueId(issue) {
      if (this.username && this.repositorio) {
        this.loader = true;
        const url = `https://api.github.com/repos/${this.username}/${this.repositorio}/issues/${issue.number}`;
        axios
          .get(url)
          .then(response => {
            this.selectedIssue = response.data;
          })
          .finally(() => {
            this.loader = false;
          })
          .catch(erro => {
            this.issueslist = [];
          });
      }
    },

    limpar() {
      (this.username = ""), (this.repositorio = ""), (this.issueslist = []);
    },

    voltar() {
      this.selectedIssue = {};
    }
  }
};
</script>

<style></style>
