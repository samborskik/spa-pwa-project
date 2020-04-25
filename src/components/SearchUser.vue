<template>
  <div>
    <input v-model="userName" />
    <button @click="handleSearchClick">
      Search
    </button>
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