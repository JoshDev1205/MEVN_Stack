<template>
  <div class="home">
    <h1 class="text-center text-muted">
      Listado de posts
    </h1>

    <div class="row">
      <div class="form-group col-xs-12">
        <label for="search">Buscar</label>
        <input v-model="search" id="search" name="search" type="text" class="form-control">
      </div>
    </div>

    <button @click="addPost()" class="pull-right btn btn-sm btn-success"> Crear post</button>

    <div class="clearfix">
      <hr>
      <div v-if="postsLength">
        <post v-for="post_data in filteredPosts" :post="post_data" key="post_data._id"></post>
      </div>
      <div class="alert alert-danger text-center" v-else> No existen posts</div>
    </div>
  </div>
</template>
<script>
import Post from './posts/SinglePost'
import { mapGetters, mapActions } from 'vuex'
export default {
  name: 'home',
  components: { Post },

  data () {
    return {
      search: ''
    }
  },
  mounted () {
    this.get_posts()
  },

  computed: {
    ...mapGetters(['postsLength']),

    filteredPosts () {
      return this.$store.getters.filteredPosts(this.search)
    }
  },

  methods: {
    addPost () {
      this.$router.push('/posts/add')
    },

    ...mapActions(['get_posts'])
  }

}
</script>
