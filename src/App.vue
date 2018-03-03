<template>
  <div id="app">
    <div>
      <div v-if="authenticated">
        <h4> Halló {{user.name}} <img :src="user.picture" /></h4>
      </div>
      <br/>
      <router-link to="/">Heim</router-link>
      <router-link v-if="authenticated" to="/peer">P2P</router-link>
      <button
        class="btn btn-primary btn-margin"
        v-if="!authenticated"
        @click="login()">
        Log In
      </button>

      <button
        class="btn btn-primary btn-margin"
        v-if="authenticated"
        @click="logout()">
        Log Out
      </button>
      <router-view
        :auth="auth"
        :authenticated="authenticated">
      </router-view>
    </div>
  </div>
</template>

<script>

import AuthService from './auth/AuthService'
const auth = new AuthService()
const { login, logout, authenticated, authNotifier } = auth

export default {
  name: 'App',
  created: function () {
    this.setUser()
  },
  data () {
    authNotifier.on('authChange', authState => {
      if (authState.authenticated) {
        this.setUser()
      } else {
        this.user = {}
      }
      this.authenticated = authState.authenticated
    })
    return {
      auth,
      authenticated,
      user: {}
    }
  },
  methods: {
    login,
    logout,
    setUser: function () {
      const obj = localStorage.getItem('user_info')
      this.user = JSON.parse(obj)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
