<template>
  <div>
    <Loader v-if="isLoading === true" />
    <b-container v-else >
      <b-row v-for="user in users" :key="user.name " class="shadow-lg favouriteUsers mb-3">
        <div>
          <b-avatar rounded="lg" size="40px"></b-avatar>
        </div>
        
        <div>
          <p>{{user.username}}</p>
        </div>
        <div>
          <p>{{user.clan || 'No clan'}}</p>
        </div>
        <div>
          <p>{{user.honor}}</p>
        </div>
        <div class="buttons">
        <div>
          <b-button variant="primary" @click="removeUsersFromFavorites(user.username)">
            <font-awesome-icon icon="user-times" />
          </b-button>
        </div>
        <div>
          <b-button>
            <font-awesome-icon icon="user-edit" />
          </b-button>
        </div>
        </div>
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
      getFavoriteUsers: async function () {
        const users = this.users;
        const db = firebase.firestore();
        const appUser = localStorage.getItem('spa-pwa-project');
        const dbRef = db.collection(appUser);
        this.isLoading = true;

        dbRef.get()
          .then(function (querySnapshot) {

            querySnapshot.forEach(function (doc) {
              users.push(doc.data())
            });
          })
          .catch(er => console.log(er))

        this.isLoading = false;
      },
      removeUsersFromFavorites: function (user) {
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
    beforeMount: async function () {
      await this.getFavoriteUsers();
    }
  }
</script>

<style scoped lang="scss">
.favouriteUsers {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  align-items: center;
}

.favouriteUsers p {
  min-width: 15vw;
}

.favouriteUsers {
  @media screen and (max-width: 576px) { 
    flex-direction: column;
    max-width: 70vw;
    margin: 0 auto;
  }
}

.buttons {
  display: flex;
  float: left;
}


</style>