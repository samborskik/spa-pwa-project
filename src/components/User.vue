<template>
  <div>
    <div class="d-block shadow-lg p-3 mb-5 bg-black rounded w-25 p-3 ml-auto mr-auto position-relative">
      <b-button class="position-absolute" @click="addUserToFavorites" id="fav-button">
        Add to favorites!
      </b-button>
      <b-avatar rounded="lg" size="72px"></b-avatar>
      <h1 id="user-data">Username:</h1>
      <p>{{ userData.username }}</p>
      <h1 id="user-data">Name:</h1>
      <p>{{ userData.name }}</p>
      <h1 id="user-data">Honor:</h1>
      <p>{{ userData.honor }}</p>
      <h1 id="user-data">Clan:</h1>
      <p>{{ userData.clan }}</p>
      <h1 id="user-data">Rank:</h1>
      <p>{{ userData.leaderboardPosition }}</p>
      
    </div>
  </div>
</template>

<script>
import firebase from 'firebase';

export default {
  name: 'User',
  props: {
    userData: Object
  },
  methods: {
    addUserToFavorites: async function() {
      const db = firebase.firestore();
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