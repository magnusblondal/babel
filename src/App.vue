<template>
  <div id="app">
    <div>
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
  data () {
    authNotifier.on('authChange', authState => {
      this.authenticated = authState.authenticated
    })
    return {
      auth,
      authenticated
    }
  },
  methods: {
    login,
    logout
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
