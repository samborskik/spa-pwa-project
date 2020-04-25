<template>
  <div>
    <p>{{ userData.username }}</p>
    <p>{{ userData.name }}</p>
    <p>{{ userData.honor }}</p>
    <p>{{ userData.clan }}</p>
    <p>{{ userData.leaderboardPosition }}</p>
    <button @click="addUserToFavorites">
      Add to favorites
    </button>
  </div>
</template>

<script>
import firebase from 'firebase';
const db = firebase.firestore();

export default {
  name: 'User',
  props: {
    userData: Object
  },
  methods: {
    addUserToFavorites: async function() {
      const appUser = localStorage.getItem('spa-pwa-project')
      const dbRef = db.collection(appUser).doc(this.userData.username);
      dbRef.get()
        .then(snapshot => {
          if (snapshot.exists) {
            alert("User already exits!")
          } 
          else {
            dbRef.set({
              username: this.userData.username,
              honor: this.userData.honor,
              clan: this.userData.clan
            })
            alert("User added!")
          }
        })
    }
  }
}
</script>