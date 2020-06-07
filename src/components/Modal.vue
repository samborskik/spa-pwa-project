<template>
  <div>
    <button @click="this.showModal">
      click to close graph
    </button>
    <bar-chart :chart-data="datacollection"></bar-chart>
  </div>
</template>

<script>
import BarChart from './Chart/Line';

export default {
  name: 'Modal',
  data: () => ({
   datacollection: {}
  }),
  components: {
    BarChart
  },
  props: {
    users: Array,
    showModal: Function
  },
  mounted () {
    this.fillData();
  },
  methods: {
    generateColor: function(string) {
      function hashCode(str) { 
          var hash = 0;
          for (var i = 0; i < str.length; i++) {
            hash = str.charCodeAt(i) + ((hash << 5) - hash);
          }
          return hash;
      } 

      function intToRGB(i){
          var c = (i & 0x00FFFFFF)
              .toString(16)
              .toUpperCase();

          return "00000".substring(0, 6 - c.length) + c;
      }
      return intToRGB(hashCode(string));
    },
    handleCloseClick: function() {
      this.changeView = !this.changeView;
    },
   
    fillData () {
      this.datacollection = {
        label: ['asd'],
        datasets: this.users.map(user => ({
          label: user.username,
          data: [user.honor],
          backgroundColor: `#${this.generateColor(user.username)}`
        })),
      }
    },
  }
}
</script>

<style scoped lang="scss">
.modal {
  // position: relative;  
}

.modal__inner {
  // position: absolute;
  top: 50%;
  left: 50%;
  width: 50vh;
  height: 50vh;
}

</style>