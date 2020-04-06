<template>
  <div id="landingPage">
    <img alt="Codewars logo" src="./assets/img/Codewars.png">
    <Login v-if="displayLogin" />
    <Signup v-else />
    <b-button @click='handleChangeFormClick'>
      <span v-if='displayLogin'>Don't have an account? Create one!</span>
      <span v-else>Have an account? Sign in</span>
    </b-button>
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

<style lang="scss">
@import './assets/styles/custom.scss';
@import '../node_modules/bootstrap/scss/bootstrap.scss';
body {
  background: #333;
  margin: 0;
  padding: 0;
}
.login {
  max-width: 20vw;
  margin: 0 auto;
}


.reddish {
  margin: 2vh;
}
#landingPage {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #b32;
  margin-top: 60px;
}
</style>
