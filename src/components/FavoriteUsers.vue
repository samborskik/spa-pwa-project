<template>
  <div>
    <div v-if="!editUser">
      <Loader v-if="isLoading === true" />
      <b-container v-else >
        <b-row v-for="user in users" :key="user.name " class="shadow-lg favouriteUsers mb-3">
          <div>
            <img :src='user.image' />
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
            <b-button @click="setEditUserImage(user.username)">
              <font-awesome-icon icon="user-edit" />
            </b-button>
          </div>
          </div>
        </b-row>
      </b-container>
    </div>
    <div v-else>
      <EditUserImage
        :user='this.focusedUser'
        :changeView='this.setEditUserImageView'
      />
    </div>
  </div>
</template>

<script>
  import Loader from './Loader';
  import EditUserImage from './EditUserImage';
  import firebase from 'firebase';

  export default {
    name: 'FavoriteUsers',
    data: () => ({
      users: [],
      focusedUser: '',
      isLoading: false,
      editUser: false,
    }),
    components: {
      Loader,
      EditUserImage
    },
    methods: {
      getFavoriteUsers: async function () {
        const users = this.users;
        const db = firebase.firestore();
        const appUser = localStorage.getItem('spa-pwa-project');

        const dbRef = db.collection(appUser);
        const storageRef = firebase.storage().ref(appUser);

        this.isLoading = true;
        await dbRef.get()
          .then(function (querySnapshot) {
            querySnapshot.forEach(async function (doc) {
              const imageRef = await storageRef
                .child(`${doc.data().username}.jpg`).getDownloadURL()
                .then(
                  url => url,
                  () => `https://robohash.org/${doc.data().username}?set=set3`
                );
              users.push({
                ...doc.data(),
                image: imageRef
                })
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
      },
      setEditUserImageView: async function () {
        this.editUser = false;
        this.isLoading = true;
        this.users = [];
        await this.getFavoriteUsers();
        this.isLoading = false;
      }, 
      setEditUserImage: function (user) {
        this.editUser = !this.editUser;
        this.focusedUser = user;
      },
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

    p {
      min-width: 15vw;
    }

    .buttons {
      display: flex;
      float: left;
    }

    img {
      width: 200px;
      height: 200px;
      object-fit: cover;
    }
    
    @media screen and (max-width: 576px) { 
      flex-direction: column;
      max-width: 70vw;
      margin: 0 auto;
    }
}
</style>
