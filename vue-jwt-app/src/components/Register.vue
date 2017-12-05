<template>
<div class="col-md-6 col-md-offset-3" id="register">

  <h1 class="text-center text-muted">
    <u>Formulario de Registro</u>
  </h1>

  <div class="alert alert-danger" v-if="error">
    Error procesando el Formulario
  </div>

  <div class="alert alert-success" v-if="success"> Te has registrado correctamente</div>

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
        <input autocomplete="off" name="password" v-model="password" v-validate data-vv-rules="required|min:6|confirmed:password_confirmation" class="form-control" type="password" id="password" placeholder="Contraseña" :class="{'has-error': errors.has('password')}">
        <span v-show="errors.has('password')" class="text-danger">
          {{ errors.first('password')}}
        </span>
      </div>
    </div>

    <div class="form-group">
      <label for="password_confirmation" class="control-label col-md-2"> Password Confirmed </label>
      <div class="col-md-10" :class="{'has-error': errors.has('password_confirmation')}">
        <input autocomplete="off" name="password_confirmation" v-model="password_confirmation" v-validate data-vv-rules="required|min:6" class="form-control" type="password" id="password" placeholder="Confirm Password" data-vv-as="Confirm Password" :class="{'has-error': errors.has('password_confirmation')}">
        <span v-show="errors.has('password_confirmation')" class="text-danger">
          {{ errors.first('password_confirmation')}}
        </span>
      </div>
    </div>
    <hr>
    <button class="btn btn-success btn-block" type="submit"> Registro </button>
  </form>
</div>
  
</template>
<script>
export default {
  name: 'register',

  data () {
    return {
      username: '',
      password: '',
      password_confirmation: '',
      error: false,
      success: false
    }
  },

  methods: {
    validateBeforeSubmit () {
      this.$validator.validateAll()
        .then(result => {
          if (!result) {

          } else {
            this.$store.dispatch('register', {
              username: this.username,
              password: this.password
            })
            .then(res => {
              console.log(res)
              this.success = true
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
