<script>
import { ref, computed } from 'vue';
export default {
  props: {
    msg: String,
  },
  setup(props) {
    const number = ref(0);

    const increment = () => {
      number.value++;
    }

    const isGreaterThen10 = computed(() => {
      return number.value > 10;
    })

    return {
      number,
      increment,
      isGreaterThen10
    }
  }
}
</script>

<template>
  <h4>{{ msg }} - {{ number }}</h4>
  <h4 v-if="isGreaterThen10">Brojot e pogolem od 10</h4>
  <h4 v-else="!isGreaterThen10">Brojot ne e pogolem od 10</h4>
  <button v-on:click="increment">Increment</button>
</template>

<style>
</style>