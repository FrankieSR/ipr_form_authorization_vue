<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Authorization />
  </div>
</template>

<script>

import Authorization from "@/components/Authorization.vue";

export default {
  name: "home",
  components: {
    Authorization
  }
};
</script>
