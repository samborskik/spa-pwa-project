<template>
  <div>
      <Loader v-if="isLoading === true"/> 
    <b-container v-else class="shadow-lg m-auto">
      <b-row v-for="user in users" :key="user.name ">
        <!-- class="shadow-lg p-3 col-12 col-md-6 col-xl-3" -->
        <b-col ><b-avatar rounded="lg" size="72px"></b-avatar></b-col>
          <b-col><p>{{user.username}}</p></b-col>
          <b-col><p>{{user.clan || 'No clan'}}</p></b-col>
          <b-col><p>{{user.honor}}</p></b-col>
          <b-col><b-button variant="primary" @click="removeUsersFromFavorites(user.username)">
            <i class="fa fa-trash">
          </b-button></b-col>
          <b-col><b-button>
            <i class="fa fa-user-edit"></i>
          </b-button></b-col>
      </b-row>
      
    </b-container>
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


