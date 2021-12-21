<template>
  <div class="container">
    <form>
      <div class="row">
        <div class="col-5">
          <input
            type="text"
            placeholder="GitHub usarname"
            class="form-control"
            id="gitUsername"
            aria-describedby="github username"
          />
        </div>
        <div class="col-5">
          <input
            type="text"
            placeholder="GitHub repositório"
            class="form-control"
            id="gitRepositorio"
            aria-describedby="git repositorio"
          />
        </div>
        <div class="col-2 d-flex">
          <button type="button" class="btn btn-success btn-sm">
            BUSCAR
          </button>
          <button type="button" class="btn btn-danger btn-sm ms-1">
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
          <tr>
            <td>Mark</td>
            <td>Otto</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "GitHubIssues",
  data() {
    return {};
  },
  methods: {}
};
</script>

<style></style>
