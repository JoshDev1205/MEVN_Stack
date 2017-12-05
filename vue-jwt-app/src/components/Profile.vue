<template>
  <div class="profile">
    <h1 class="text-centered text-muted">
      <ul>Perfil de usuario</ul>
    </h1>


    <div class="well">
      <p>Id: {{ user._id }}</p>
      <p>Username: {{ user.username }}</p>
      <p>Token: {{ token }}</p>

    </div>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
export default {
  name: 'profile',

  computed: {
    ...mapGetters([
      'user'
    ]),

    token () {
      return window.localStorage.getItem('_token')
    }
  }
}
</script>
<style scoped>
.well {
  word-wrap: break-word;
}
</style>
