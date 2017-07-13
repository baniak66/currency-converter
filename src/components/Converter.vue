<template>
  <div class="converter">
    <h2>Vue.js currency converter</h2>
    <input id="given" type="text" v-model="givenAmount"> PLN
    <multiselect v-model="value" :options="rates" :custom-label="codeWithCurr" placeholder="Select one" label="name" ></multiselect>
    <h3>{{converted}}</h3>
  </div>
</template>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style>
  .multiselect{
    width: 20%;
    text-align: center !important;
  }
  #given{
    font-family: inherit;
    font-size: 14px;
    height: 35px;
    border:1px solid #cccccc;
    border-radius: 5px;
  }
</style>

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
      converted: function () {
        return this.givenAmount * this.value.mid + " " + this.value.code
      }
    }
  }
</script>
