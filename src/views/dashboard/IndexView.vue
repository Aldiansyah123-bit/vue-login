<template>
  <div class="container-fluid mt-5">
    <div class="row">
      <div class="col-md-4">
        <div class="card">
          <div class="card-body">
            Main Menu 
            <hr>
            <ul class="list-group">
              <router-link :to="{name: 'dashboard'}" class="list-group-item text-dark text-decoration-none">Dashboard</router-link>
              <li @click.prevent="logout" class="list-group-item text-dark text-decoration-none" style="cursor:pointer">Logout</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="col-md-8">
        <div class="card">
          <div class="card-body">
            Dashboard
            <hr>
            Selamat Datang <strong>{{ user.name }}</strong>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
  setup() {
    const token = localStorage.getItem('token')

    const router = useRouter()

    const user = ref('')

    onMounted(() => {
      if (!token) {
        return router.push({
          name: 'login'
        })
      }

      axios.defaults.headers.common.Authorization = `Bearer ${token}`
      axios.get('http://localhost:8000/api/user')
      .then(response => {

        user.value = response.data

      })
      .catch(error => {
        console.log(error.response.data)
      })
    })

    function logout() {
      axios.defaults.headers.common.Authorization = `Bearer ${token}`
      axios.post('http://localhost:8000/api/logout')
      .then(response => {
        if (response.data.success) {
          localStorage.removeItem('token') 
          return router.push({
            name: 'login'
          })         
        }
      })
      .catch(error => {
        console.log(error.response.data)
      })
    }

    return {
      token,
      user,
      logout
    }
  }  
}
</script>
