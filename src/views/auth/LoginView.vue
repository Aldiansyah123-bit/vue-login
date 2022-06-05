<template>
  <div class="container-fluid mt-5">
    <div class="row justify-content-center">
      <div class="col-md-4">
        <div class="alert alert-danger" v-if="loginFailed">
          Email atau Password anda salah.
        </div>
        <div class="card border-0 rounded-shadow">
          <div class="card-body">
            <h4>Login</h4>
            <hr>
            <form @submit.prevent="login">
              <div class="form-group">
                <label for="">Email Address</label>
                <input type="email" v-model="user.email" class="form-control" placeholder="Email Address">
              </div>
              <div class="mt-2 alert alert-danger" v-if="validation.email">
                {{ validation.email[0] }}
              </div>
              <div class="form-group">
                <label for="">Password</label>
                <input type="password" v-model="user.password" class="form-control" placeholder="Password">
              </div>
              <div class="mt-2 alert alert-danger" v-if="validation.password">
                {{ validation.password[0] }}
              </div>
              <button type="submit" class="btn btn-primary btn-block">Login</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
  setup() {
    const router = useRouter()

    const user = reactive({
      email: '',
      password: '',
    })

    const validation = ref([])

    const loginFailed = ref(null)

    function login(){
      let email = user.email
      let password = user.password

      axios.post('http://127.0.0.1:8000/api/login',{
        email,
        password,
      })
      .then(response => {
        if(response.data.success){
          localStorage.setItem('token', response.data.token)

          return router.push({
            name: 'dashboard'
          })
        }

        loginFailed.value = true

      }).catch(error => {

        validation.value = error.response.data
        
      })
    }

    return {
      user,
      validation,
      loginFailed,
      login
    }
  }
}
</script>
