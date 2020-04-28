<template>
  <div>
    
     <b-input-group class="mt-4-d-inline-flex p-2 bd-highlight w-25 p-3 ml-auto mr-auto">
    <b-form-input v-model="userName"></b-form-input>
    <b-input-group-append>
      <b-button class="raddish" variant="primary" @click="handleSearchClick">Search</b-button>
    </b-input-group-append>
  </b-input-group>

  </div>


</template>

<script>

export default {
  name: 'SearchUser',
  props: {
    getUserData: Function
  },
  data: () => ({
    userName: "",
  }),
  methods: {
    handleSearchClick: function() {
      this.getUserData(this.userName)
    }
  }
}
</script>