<template>
  <div>
    <Loader v-if="isLoading === true"/>
    <div v-else>
      <ul>
        <li v-for="user in users" :key="user.name">
          <p>{{user.username}}</p>
          <p>{{user.clan}}</p>
          <p>{{user.honor}}</p>
          <button @click="removeUsersFromFavorites(user.username)">
            Remove
          </button>
        </li>
      </ul>
      
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
    isLoading: false,
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
      this.isLoading = true;

      dbRef.get()
        .then(function (querySnapshot) {
          
          querySnapshot.forEach(function(doc) {
            users.push(doc.data())
          });
        })
        .catch(er => console.log(er))

      this.isLoading = false;
    },
    removeUsersFromFavorites: function(user) {
      const db = firebase.firestore();
      const appUser = localStorage.getItem('spa-pwa-project');
      const dbRef = db.collection(appUser);
      this.isLoading = true;

      dbRef.doc(user).delete()
        .then(alert("User deleted"))
        .catch(er => console.log(er))

      const index = this.users.indexOf(user);
      this.users.splice(index, 1);

      this.isLoading = false;
    }
  },
  beforeMount: async function() {
    await this.getFavoriteUsers();
  }
}
</script>