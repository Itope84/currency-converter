<template>
  <div id="app">
    <div class="top-slanted"></div>
    <div class="body">
      <div class="app-holder">
        <h1 class="h5 header-font app-title">Currency Converter</h1>
        <p class="description">Enter any amount below and we'll convert it to your selected currency</p>

        <div class="converter">
          <div class>
            <div style="justify-content: inherit; margin-bottom: 0" class="field has-addons has-addons-right">
              <div class="control">
                <currency-options :currencies="currencies" v-model="activeCurrency"></currency-options>
              </div>
              <p class="control" style="flex: 1">
                <input class="input" type="number" v-model="amount" placeholder="Enter Amount" />
              </p>
            </div>

            <div class="converted-value">
              <div
                class="d-flex justify-content-center align-items-center"
                style="min-height: 60px;"
                v-if="!rates || !amount"
              >
                <fade-loader color="#e5e5e5"></fade-loader>
              </div>

              <h1 v-else class="value">
                &#8358;{{convertedValue}}
              </h1>
            </div>
          </div>
        </div>

        <div style="margin-top: 100px;">
          <rates-card @updatedRates="updateRates" :currencies="currencies" v-model="activeCurrency"></rates-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CurrencyOptions from "./components/CurrencyOptions.vue";
import RatesCard from "./components/RatesCard.vue";
import { FadeLoader } from "@saeris/vue-spinners";
const currencies = [
  {
    name: "EUR",
    flag: "/img/eur.png"
  },
  {
    name: "GBP",
    flag: "/img/gbp.png"
  },

  {
    name: "USD",
    flag: "/img/usd.png"
  }
];
export default {
  name: "app",
  components: {
    RatesCard,
    FadeLoader,
    CurrencyOptions
  },
  data() {
    return {
      activeCurrency: currencies[0],
      currencies,
      rates: null,
      amount: null
    };
  },

  computed: {
    convertedValue() {
      return (parseInt(this.amount) * parseFloat(this.rates[this.activeCurrency.name + "_NGN"])).toFixed(2);
    }
  },

  methods: {
    updateRates(rates) {
    this.rates = rates;
  }
  }
};
</script>

<style lang="scss">
// $secondary-color:

*:focus {
  border: inherit !important;
}
#app {
  font-family: "Montserrat", sans-serif;
  margin-top: 0px;
}

.body {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
}

.app-holder {
  margin-left: auto;
  margin-right: auto;
  margin-top: 50px;
  max-width: 400px;
  width: 90%;
}

.app-title {
  font-size: 1.5rem;
  text-align: center;
  font-weight: 500;
  color: #fff;
}

.converter {
  margin-top: 50px;
}

.currency-from {
  button {
    border-top-right-radius: 0 !important;
    border-bottom-right-radius: 0 !important;
    &:focus {
      border: none !important;
      box-shadow: inherit !important;
    }
  }
}

.header-font {
  // font-family: 'Roboto', sans-serif;
}

.d-flex {
  display: flex;
}

.justify-content-center {
  justify-content: center;
}

.align-items-center {
  align-items: center;
}

.top-slanted {
  width: 100vw;
  height: 500px;
  background: linear-gradient(270deg, #2dc8f3 0, #1497e2 100%);
  z-index: 1;
  -webkit-clip-path: polygon(0 0, 0 100%, 100% 90%, 100% 0);

  @media (max-width: 420px) {
    height: 65vh;
  }
}

.currency-item {
  display: flex;
  align-items: center;
}

.currency-icon {
  width: 50px;
  height: 30px;
  margin-right: 5px;
  display: inline-block;

  img.img-fluid {
    max-width: 100%;
    height: auto;
  }
}

.control {
  height: 50px;
  .currency-from,
  .input,
  .button {
    height: 100%;
  }
}

.bg-main {
  background-color: #1497e2 !important;
}

.converted-value {
  margin-left: auto;
  margin-right: auto;
  background-color: #fff;
  width: 90%;
  color: #1497e2;
  text-align: center;
  box-shadow: 0px 3px 55px 24px rgba(34, 48, 73, 0.3);
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  padding: 1rem 0px;
  overflow: hidden;

  .value {
    font-size: 35px;
    font-weight: 600;
  }
}

.description {
  color: #fff;
  font-size: 80%;
  text-align: center;
}

.ml-auto {
  margin-left: auto;
}
</style>
