<template>
  <div>
    <div class="col-12 col-sm-8 col-lg-3 m-auto p-3 shadow-lg">
      <b-button class="position-absolute" @click="addUserToFavorites" id="fav-button">
        Add to favorites!
      </b-button>
      <b-avatar rounded="lg" size="72px"></b-avatar>
      <h1>Username:</h1>
      <p>{{ userData.username }}</p>
      <h1>Name:</h1>
      <p>{{ userData.name }}</p>
      <h1>Honor:</h1>
      <p>{{ userData.honor }}</p>
      <h1>Clan:</h1>
      <p>{{ userData.clan }}</p>
      <h1>Rank:</h1>
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

<style scoped lang="scss">
  #fav-button {
    right: 0;
    top: 0;
  }
</style>