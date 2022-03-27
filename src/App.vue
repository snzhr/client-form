<template>
  <div class="app">
    <UserForm />
  </div>
</template>

<script>
import UserForm from "./components/UserForm.vue";
export default {
  name: "App",
  components: { UserForm },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}
body {
  padding: 0;
  margin: 0;
  font: {
    family: "Rubik", sans-serif;
    weight: 500;
  }
}
</style>
