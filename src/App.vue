<template>
  <div class="container" id="app">
    <div class="mb-5">
      <img id="logo-img"
        src="./assets/GitHub-Mark.png"
      />

      <h2 class="text-center">GitHub Issues</h2>
    </div>


    <router-view />
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#logo-img{
  width: 200px;
  display: block;
  background: url('./assets/GitHub-Mark.png');
  margin: 0 auto;
}

.body{
background: url('./assets/GitHub-Mark.png');
background-size: cover;
}

</style>
