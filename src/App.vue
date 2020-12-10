<template>
 <div id="app">
    <nav>
      <div>
        <router-link to="/">
          Okta-Vue Sample Project
        </router-link>
        <router-link to="/login" v-if="!authenticated">
          Login
        </router-link>
        <router-link to="/profile" v-if="authenticated" >
          Profile
        </router-link>
        <router-link to="/" v-if="authenticated" v-on:click.native="logout()">
          Logout
        </router-link>
      </div>
    </nav>
    <div id="content">
      <router-view/>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data: function () {
      return { authenticated: false }
    },
    created () { this.isAuthenticated() },
    watch: {
      // Everytime the route changes, check for auth status
      '$route': 'isAuthenticated'
    },
    methods: {
      async isAuthenticated () {
        this.authenticated = await this.$auth.isAuthenticated()
      },
      async logout () {
        await this.$auth.logout()
        await this.isAuthenticated()
      }
    }
  }
</script>

<style>
nav div a {margin-right:10px}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
