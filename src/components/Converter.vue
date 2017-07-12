<template>
  <div class="converter">
    <h2>Vue.js currency converter</h2>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'converter',
    data () {
    },
    created: function () {
      this.getRates();
    },
    methods: {
      getRates: function () {
        axios.get('http://api.nbp.pl/api/exchangerates/tables/a/')
          .then(function (response) {
            console.log(response.data[0].no)
          })
          .catch(function (error) {
            console.log(error)
          })
      }
    },
    computed: {

    }
  }
</script>
