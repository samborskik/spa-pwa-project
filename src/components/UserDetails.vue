<template>
  <div>
    <Loader v-if="isLoading" />
    <div v-else>
      <div>
        <p>{{this.userDetails.username}}</p>
        <p>{{this.userDetails.name}}</p>
        <p>{{this.userDetails.honor}}</p>
        <p>{{this.userDetails.clan}}</p>
        <p>{{this.userDetails.leaderboardPosition}}</p>
        <ul v-for="[key, value] of Object.entries(userDetails.ranks.languages)" :key="key">
          <li>
            {{key}} 
            {{value.score}}
            {{value.rank*(-1)}} kyu
          </li>
        </ul>

      </div>
      <button @click="this.backCallback">
        backToFav users
      </button>
    </div>
  </div>
</template>

<script>
import Loader from './Loader';

export default {
  name: 'UserDetails',
  props: {
    backCallback: Function,
    user: String
  },
  components: {
    Loader
  },
  data: () => ({
    isLoading: false,
    userDetails: {}
  }),
  methods: {
    getUserData: async function() {
      this.isLoading = true;
      await fetch(`https://cors-anywhere.herokuapp.com/https://www.codewars.com/api/v1/users/${this.user}`)
        .then(res => res.json())
        .then(res => {
          this.userDetails = res
          this.isLoading = false;
        })

    },
  },
  mounted: async function() {
    await this.getUserData();
    console.log(this.userDetails)

  }
}
</script>