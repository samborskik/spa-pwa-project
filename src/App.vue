<template>
  <div id="landingPage">
    <HomePage
      :setUserSession="this.setUserSession"
      v-if="isUserLoggedIn"
    />
    <div v-else>
      <img alt="Codewars logo" src="./assets/img/Codewars.png">
      <Login
        :setUserSession="this.setUserSession"
        v-if="displayLogin"
      />
      <Signup v-else />
      <b-button @click='handleChangeFormClick'>
        <span v-if='displayLogin'>Don't have an account? Create one!</span>
        <span v-else>Have an account? Sign in</span>
      </b-button>
      <b-button @click='handleLoginWithGoogle'>
        <span>Login with Google account</span>
      </b-button>
    </div>

  </div>
</template>

<script>
import Login from './components/Login.vue';
import Signup from './components/Signup.vue';
import HomePage from './components/HomePage.vue';

import firebase from 'firebase';

export default {
  name: 'App',
  components: {
    Login,
    Signup,
    HomePage,
  },
  data: () => ({
      displayLogin: true,
      isUserLoggedIn: false
  }),
  methods: {
    handleChangeFormClick: function() {
      this.displayLogin = !this.displayLogin;
    },
    checkUserSession: function() {
      let session = localStorage.getItem('spa-pwa-project')
      this.isUserLoggedIn = null !== session;
    },
    setUserSession: function(param) {
      this.isUserLoggedIn = param;
    },
    handleLoginWithGoogle: function() {
      var provider = new firebase.auth.GoogleAuthProvider();
      firebase.auth().signInWithPopup(provider).then((result) => {
      localStorage.setItem('spa-pwa-project', result.additionalUserInfo.profile.email);
      this.setUserSession(true);
      alert('Logged in!')
    }).catch((err) => console.log(err))

    }
  },
  mounted: function() {
    this.checkUserSession();
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
