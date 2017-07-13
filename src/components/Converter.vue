<template>
  <div class="converter">
    <h2>Vue.js currency converter</h2>
    <multiselect v-model="value" :options="rates" :custom-label="codeWithCurr" placeholder="Select one" label="name" ></multiselect>
  </div>
</template>

<script>
  import axios from 'axios';
  import Multiselect from 'vue-multiselect';

  export default {
    components: {
      Multiselect
    },
    name: 'converter',
    data () {
      return {
        rates: [],
        givenAmount: '',
        value: { code: '', currency: '', mid: '' },
      }
    },
    created: function () {
      this.getRates();
    },
    methods: {
      getRates: function () {
        var vm = this;
        axios.get('http://api.nbp.pl/api/exchangerates/tables/a/')
          .then(function (response) {
            vm.rates = response.data[0].rates
            console.log(response.data[0].rates)
          })
          .catch(function (error) {
            console.log(error)
          })
      },
      codeWithCurr ({ code, currency }) {
        if (code == '') {
          return "Choose currency..."
        } else {
          return `${currency} — [${code}]`
        }
      }
    },
    computed: {

    }
  }
</script>
