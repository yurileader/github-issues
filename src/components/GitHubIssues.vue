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

    <div class="mt-5">
      <table class="table table-hover">
        <thead>
          <tr>
            <th scope="col">Número</th>
            <th scope="col">Título</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="usr in userlist" :key="usr.id">
            <td>{{ usr.id }}</td>
            <td>{{ usr.body }}</td>
          </tr>
          <tr v-if="!userlist">
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
      userlist: []
    };
  },

  methods: {
    buscarIssues() {
      console.log("BUSCOU");
      axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then(response => {
          this.userlist = [...response.data];
          console.log(this.userlist);
        })
        .catch(erro => console.log(erro));
    },

    limpar() {
      console.log("Limpou");
      (this.username = ""), (this.repositorio = ""), (this.userlist = []);
    }
  }
};
</script>

<style></style>
