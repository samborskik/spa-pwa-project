<template>
  <div>
    <Loader v-if="isLoading"/>
    <div v-else>
      <p>{{this.users}}</p>
    </div>
  </div>
</template>

<script>
import Loader from './Loader';
import firebase from 'firebase';

export default {
  name: 'FavoriteUsers',
  data: () => ({
    users: [],
    isLoading: true,
  }),
  components: {
    Loader
  },
  methods: {
    getFavoriteUsers: async function() {
      const users = this.users;
      const db = firebase.firestore();
      const appUser = localStorage.getItem('spa-pwa-project');
      const dbRef = db.collection(appUser);
      dbRef.get()
        .then(function (querySnapshot) {
          querySnapshot.forEach(function(doc) {
            users.push(doc.data())
            console.log(doc.data())
          });
        })
        .catch(er => console.log(er))
    },
    removeUsersFromFavorites: function(user) {
      const db = firebase.firestore();
      const appUser = localStorage.getItem('spa-pwa-project');
      const dbRef = db.collection(appUser);
      dbRef.doc(user).delete()
        .then(alert("User deleted"))
        .catch(er => console.log(er))
    }
  },
  beforeMount: async function() {
    await this.getFavoriteUsers();
      this.isLoading = false;
  }
}
</script>