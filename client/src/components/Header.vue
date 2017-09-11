<template>
  <v-toolbar fixed class="deep-purple darken-4">
    <v-toolbar-title class="mr-4">
      <span 
        class="home"
        @click="navigateTo({name: 'root'})">
        TabTracker
      </span>
    </v-toolbar-title>

    <v-toolbar-items>
      <v-btn flat dark
        v-if="!$store.state.isUserLoggedIn"
        @click="navigateTo({name: 'songs'})">
        Browser
      </v-btn>
    </v-toolbar-items>

    <v-spacer></v-spacer>

    <v-toolbar-items>
      <v-btn flat dark
        v-if="!$store.state.isUserLoggedIn"
        @click="navigateTo({name: 'login'})">
        Login
      </v-btn>

      <v-btn flat dark
        v-if="!$store.state.isUserLoggedIn"
        @click="navigateTo({name: 'register'})">
        Sign Up
      </v-btn>

      <v-btn flat dark
        v-if="$store.state.isUserLoggedIn"
        @click="logout">
        Log Out
      </v-btn>
    </v-toolbar-items>
  </v-toolbar>
</template>

<script>
export default {
  methods: {
    navigateTo (route) {
      this.$router.push(route)
    },
    logout () {
      this.$store.dispatch('setToken', null)
      this.$store.dispatch('setUser', null)
      // TODO: redirect to home page
      this.$router.push({
        name: 'root'
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .home {
    cursor: pointer;
  }
  .home:hover {
    color: #d1c4e9;
  }
</style>
