<template>
  <div class="login">
    <h1>Sign in</h1>
    <input type="text" v-model="email" placeholder="Email">
    <input type="password" v-model="password" placeholder="Password">
    <button @click="signUp" >Log in!</button>
  </div>
</template>

<script>
import firebase from 'firebase';


export default {
  name: 'Login',
  data: () => ({
    email: '',
    password: '',
  }),
  methods: {
    signUp: function() {
      firebase.auth().signInWithEmailAndPassword(this.email, this.password)
        .then(
          () => alert("Loged in!"),
          (err) => alert(`Error: ${err.message}`)
        )
    }
  }
}
</script>