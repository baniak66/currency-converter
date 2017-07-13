<template>
  <div class="converter">
    <h2>Vue.js currency converter</h2>
    <input id="given" type="text" v-model="givenAmount" placeholder="Amount to convert..."> PLN
    <div>
    <multiselect v-model="value" :options="rates" :custom-label="codeWithCurr" placeholder="Select one" label="name" ></multiselect>
    </div>
    <h2>{{converted}}</h2>
    <p>according to NBP table no. {{table}} [{{date}}]</p>
  </div>
</template>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style>
  .multiselect{
    display:inline-block;
    width: 20%;
  }
  #given{
    margin-bottom: 20px;
    padding-left: 10px;
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
        table: '',
        date: ''
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
            var info = response.data[0]
            vm.rates = info.rates
            vm.table = info.no
            vm.date = info.effectiveDate
            console.log(response)
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
        var result = (this.givenAmount * this.value.mid).toFixed(2) + " " + this.value.code;
        var re = new RegExp("^NaN");
        if (re.test(result)) {
          return "You give wrong amount.";
        } else {
          return result;
        }
      }
    }
  }
</script>
