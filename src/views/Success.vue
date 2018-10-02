<template>
  <div class="">
      <img alt="Vue logo" src="../assets/logo.png">
    <h1>Hello {{username}}</h1>
  </div>
</template>

<script>
// @ is an alias to /src


export default {
  name: "home",
  data: function() {
      return {
          username: localStorage.username
      }
  },
  components: {
  },
};
</script>
