<template>
  <div id="landingPage">
    <img alt="Vue logo" src="./assets/logo.png">
    <Login v-if="displayLogin" />
    <Signup v-else />
    <button @click='handleChangeFormClick'>
      <span v-if='displayLogin'>Don't have an account? Signup insted</span>
      <span v-else>Have an account? Sign in</span>
    </button>
  </div>
</template>

<script>
import Login from './components/Login.vue';
import Signup from './components/Signup.vue';

export default {
  name: 'App',
  components: {
    Login,
    Signup
  },
  data: () => ({
      displayLogin: true,
  }),
  methods: {
    handleChangeFormClick: function() {
      this.displayLogin = !this.displayLogin;
    }
  }
}

</script>

<style>
#landingPage {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
