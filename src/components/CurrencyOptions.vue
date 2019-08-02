<template>
  <div
    class="dropdown currency-from"
    :class="{'is-active': showOptions}"
    v-on-clickaway="closeOptions"
  >
    <div class="dropdown-trigger">
      <button
        class="button"
        aria-haspopup="true"
        aria-controls="dropdown-menu"
        @click="showOptions = !showOptions"
      >
        <span class="currency-item"> <span class="currency-icon"><img :src="value.flag" class="img-fluid" alt=""></span> {{value.name}}</span>
        <span class="icon is-small">
          <i class="fas fa-angle-down" aria-hidden="true"></i>
        </span>
      </button>
    </div>
    <div class="dropdown-menu" id="dropdown-menu" role="menu">
      <div class="dropdown-content">
        <a href="#" class="dropdown-item" v-for="(option, index) in currencies" @click.prevent="select(option)" :key="index" :class="{'is-active': option.name === value.name}">
            <span class="currency-item"> <span class="currency-icon"><img :src="option.flag" class="img-fluid" alt=""></span> {{option.name}}</span>
        </a>
        <!-- <a href="#" class="dropdown-item">Other dropdown item</a> -->
        <!-- <hr class="dropdown-divider" /> -->
        <!-- <a href="#" class="dropdown-item">With a divider</a> -->
      </div>
    </div>
  </div>
</template>
<script>
import { mixin as clickaway } from "vue-clickaway";
export default {
  mixins: [clickaway],

  props: {
      currencies: {
          type: Array,
          required: true
      },
      value: {
          type: Object,
          required: true
      }
  },

  model: {
      prop: "value",
      event: "input"
  },

  data() {
    return {
      showOptions: false,
    };
  },

  methods: {
    closeOptions() {
      this.showOptions = false;
    },
    select (option) {
        this.$emit('input', option)
        this.closeOptions()
    }
  }
};
</script>

<style lang="scss">

</style>
