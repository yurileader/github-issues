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

    <div class="mt-5" v-if="!selectedIssue.id">
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
              <img src="../assets/Spinner-animado.svg" alt="" />
            </td>
          </tr>
          <tr v-for="usr in userlist" :key="usr.id">
            <td>
              <a @click.prevent.stop="buscarIssueId(usr.number)"
              class="text-decoration-none fw-bold" href="">{{
                usr.number
              }}</a>
            </td>
            <td>{{ usr.title }}</td>
          </tr>
          <tr v-if="!userlist.length">
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
      user: !{
        id: Number,
        userId: Number,
        title: String,
        body: String
      },
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
        console.log(url);
        axios
          .get(url)
          .then(response => {
            this.userlist = [...response.data];
            console.log(this.userlist);
          })
          .finally(() => {
            this.loader = false;
          })
          .catch(erro => {
            this.userlist = [];
          });
      }
    },

    buscarIssueId(issueId) {
      if (this.username && this.repositorio) {
        this.loader = true;
        const url = `https://api.github.com/repos/${this.username}/${this.repositorio}/issues/${issueId}`;
        axios
          .get(url)
          .then(response => {
            this.selectedIssue = response.data;
          })
          .finally(() => {
            this.loader = false;
          })
          .catch(erro => {
            this.userlist = [];
          });
      }
    },

    limpar() {
      (this.username = ""),
        (this.repositorio = ""),
        (this.userlist = []);
    },

    voltar(){
    (this.selectedIssue = {});
    }
  }
};
</script>

<style></style>
