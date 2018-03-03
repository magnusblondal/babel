<template>
  <div>
    <h4 v-if="authenticated">
      You are logged in!
    </h4>
    <h4 v-if="!authenticated">
      You are not logged in! Please <a @click="auth.login()">Log In</a> to continue.
    </h4>
    <div v-if="authenticated">
    </div>
    {{foo}}
    <br />
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'home',
  props: ['auth', 'authenticated'],
  data () {
    return {
      foo: 'foo'
    }
  },
  computed: {
  },
  created: function () {
    const self = this
    const token = localStorage.getItem('access_token')
    axios.get('https://babel.eu.auth0.com/userinfo', {'headers': { 'Authorization': 'Bearer ' + token }})
      .then(function (response) {
        console.log(response)
        self.foo = response.data.sub
      })
      .catch(function (error) {
        console.log(error)
      })
  },
  methods: {
  }
}
</script>

<style>
</style>
