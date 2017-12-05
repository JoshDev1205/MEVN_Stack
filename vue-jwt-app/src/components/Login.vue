<template>
  <div class="login col-md-6 col-md-offset-3">
    <h1 class="text-center text-muted">
      <u>Iniciar sesión</u>
    </h1>

    <div class="alert alert-danger" v-if="error">
      Error procesando el formulario
    </div>

    <hr />

  <form autocomplete="off" class="form-horizontal" @submit.prevent="validateBeforeSubmit">
    <div class="form-group">
      <label for="username" class="control-label col-md-2"> Username </label>
      <div class="col-md-10" :class="{'has-error': errors.has('username')}">
        <input autocomplete="off" name="username" v-model="username" v-validate data-vv-rules="required|min:2" class="form-control" type="text" id="username" placeholder="Nombre de usuario" :class="{'has-error': errors.has('username')}">
        <span v-show="errors.has('username')" class="text-danger">
          {{ errors.first('username')}}
        </span>
      </div>
    </div>

    <div class="form-group">
      <label for="password" class="control-label col-md-2"> Password </label>
      <div class="col-md-10" :class="{'has-error': errors.has('password')}">
        <input autocomplete="off" name="password" v-model="password" v-validate data-vv-rules="required|min:6" class="form-control" type="password" id="password" placeholder="Contraseña" :class="{'has-error': errors.has('password')}">
        <span v-show="errors.has('password')" class="text-danger">
          {{ errors.first('password')}}
        </span>
      </div>
    </div>
    <hr>
    <button class="btn btn-success btn-block" type="submit"> Iniciar Sesión </button>
  </form>

</div>
  
</template>

<script>
import * as types from '@/mutations_types'
export default {
  name: 'login',
  data () {
    return {
      username: '',
      password: '',
      error: null
    }
  },
  methods: {
    validateBeforeSubmit () {
      this.$validator.validateAll()
        .then(result => {
          if (!result) {

          } else {
            this.$store.dispatch('login', {
              username: this.username,
              password: this.password
            })
            .then(res => {
              localStorage.setItem('_token', res.data)
              this.$store.commit(types.SET_USER)
              this.$router.push('/')
            }, err => {
              console.log(err)
              this.error = true
            })
          }
        })
        .catch(error => {
          console.log(error.msg)
        })
    }
  }
}
</script>
