<template>
  <div class="col-md-12">

    <div class="alert alert-danger" v-if="error">
      Error procesando el formulario
    </div>

    <post-form
      v-if="post"
      v-bind:post="post"
      v-on:goBack="goBack()"
      v-on:submitPost="updatePost()"
      v-bind:submitText="'Actualizar Post'"
      v-bind:goBackText="'Volver'"
    />

  </div>
</template>
<script>
import PostForm from '@/components/posts/PostForm'
export default {
  name: 'post-edit',
  components: {
    PostForm
  },
  data () {
    return {
      error: false
    }
  },

  mounted () {
    if (this.$store.state.postsModule.posts.length > 0) {
      var post = this.$store.getters.findPost(this.$route.params.id)
      this.$store.commit('SET_POST', post[0])
    } else {
      this.$store.dispatch('get_post', this.$route.params.id)
      .then(res => console.log(res)
    )
    }
  },

  computed: {
    post () {
      return this.$store.state.postsModule.current_post
    }
  },

  methods: {
    updatePost () {
      this.$store.dispatch('update_post', { post: this.post })
        .then(res => {
          this.$router.push(`/post/${this.post._id}`)
        }, err => {
          console.log(err)
          this.error = true
        })
    },

    goBack () {
      this.$router.push(`/post/${this.post._id}`)
    }
  }
}
</script>
