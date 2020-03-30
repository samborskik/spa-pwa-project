<template>
  <div class="login">
    <h1>Create an account</h1>
    <input type="text" v-model="email" placeholder="Email">
    <input type="password" v-model="password" placeholder="Password">
    <button @click="signUp" >Sign up!</button>
  </div>
</template>

<script>
import firebase from 'firebase';


export default {
  name: 'Signup',
  data: () => ({
    email: '',
    password: '',
  }),
  methods: {
    signUp: function() {
      firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
        .then(
          () => alert("User created!"),
          (err) => alert(`Error: ${err.message}`)
        )
    }
  }
}
</script>