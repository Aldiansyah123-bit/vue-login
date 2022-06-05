<template>
  <div class="container-fluid mt-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card boder-0 rounded shadow">
          <div class="card-body">
            <h4>Register</h4>
            <hr>
            <form @submit.prevent="register">
              <div class="row">
                <div class="col-md-6">
                  <div class="form-group">
                    <label for="">Full Name</label>
                    <input type="text" v-model="user.name" class="form-control" placeholder="Fullname">
                  </div>
                  <div class="mt-2 alert alert-danger" v-if="validation.name">
                    {{ validation.name[0] }}
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="form-group">
                    <label for="">Email Address</label>
                    <input type="email" v-model="user.email" class="form-control" placeholder="Email Address">
                  </div>
                  <div class="mt-2 alert alert-danger" v-if="validation.email">
                    {{ validation.email }}
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-md-6">
                  <div class="form-group">
                    <label for="">Password</label>
                    <input type="password" v-model="user.password" class="form-control" placeholder="Password">
                  </div>
                  <div v-if="validation.password" class="mt-2 alert alert-danger">
                    {{ validation.password[0] }}
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="form-group">
                    <label for="">Konfirmasi Password</label>
                    <input type="password" v-model="user.password_confirmation" class="form-control" placeholder="Konfirmasi Password">
                  </div>
                </div>
              </div>
              <button type="submit" class="btn btn-primary btn-block">Register</button>
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
      name: '',
      email: '',
      password: '',
      password_confirmation: ''
    })

    const validation = ref([])

    function register() {
      let name = user.name
      let email = user.email
      let password = user.password
      let password_confirmation = user.password_confirmation

      axios.post('http://127.0.0.1:8000/api/register',{
        name,
        email,
        password,
        password_confirmation
      })

      .then(() => {

        return router.push({
          name: 'login'
        })

      }).catch(error => {
          validation.value = error.response.data
      })
    }
    return {
      user,
      validation,
      register
    }
  }
}
</script>
