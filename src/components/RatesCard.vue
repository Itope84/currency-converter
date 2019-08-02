<template>
  <div class="card rates-container">
    <div
      class="d-flex justify-content-center align-items-center"
      style="min-height: 200px;"
      v-if="loading || !rates"
    >
      <bounce-loader color="#e5e5e5"></bounce-loader>
    </div>

    <div v-else>
      <div class="d-flex currency-row" @click="select(currency)" :class="{'active': currency.name === activeCurrency.name}" v-for="currency in currencies" :key="currency.name">
        <span class="currency-item">
          <span class="currency-icon">
            <img :src="currency.flag" class="img-fluid" alt />
          </span>
          {{currency.name}}
        </span>

        <div class="ml-auto">
            &#8358;{{rates[currency.name + '_NGN'].toFixed(2)}} / {{currency.name}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { BounceLoader } from "@saeris/vue-spinners";
export default {
  props: {
    currencies: {
      type: Array,
      required: true
    },
    activeCurrency: {
      type: Object,
      required: true
    }
  },
  model: {
      prop: "activeCurrency",
      event: "input"
  },
  data() {
    return {
      rates: null,
      loading: false
    };
  },

  components: {
    BounceLoader
  },

  methods: {
      select (option) {
        this.$emit('input', option)
        this.closeOptions()
    },
    fetchRates() {
      this.loading = true;
      const APIKEY = "7498172954f8bc251ab1";
      const currenciesStr = this.currencies.reduce(
        (prevStr, currency) =>
          `${prevStr ? prevStr + "," : prevStr}${currency.name}_NGN`,
        ""
      );
      const url = `https://free.currconv.com/api/v7/convert?q=${currenciesStr}&compact=ultra&apiKey=${APIKEY}`;
      axios
        .get(url)
        .then(response => {
            this.$emit('updatedRates', response.data)
          this.rates = response.data;
        })
        .finally(() => {
          this.loading = false;
        });
    }
  },

  mounted() {
    this.fetchRates();
  }
};
</script>

<style lang="scss">
.rates-container {
  min-height: 300px;
  box-shadow: 0px 3px 55px 24px rgba(34, 48, 73, 0.3);
  border-radius: 5px;
  padding: 1rem 0px;
}

.currency-row {
  height: 70px;
  padding: 0px 40px;
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-top: 5px;

  &.active, &:hover {
      background-color: #2a9ae2;
      color: #fff;
  }
}
</style>
