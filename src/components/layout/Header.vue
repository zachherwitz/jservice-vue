<template>
  <header class="header">
    <router-link class="link" to="/">Home</router-link>
    <router-link class="link" to="/about">About</router-link>
  </header>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style scoped>
header {
  background: #eef4ed;
  display: flex;
  align-items: center;
  justify-content: center;
}

.link {
  margin: 5px;
  color: #134074;
}
</style>
