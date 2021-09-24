<script>
export default {
  props: {
    msg: String
  },
  created() {
    console.log('created')
  },
  mounted() {
    console.log('mounted')
  },
  data() {
    return {
      value: 0,
    }
  },
  methods: {
    increment() {
      this.value++;
    },
  },
  computed: {
    isGreatedThan10() {
      return this.value > 10;
    }
  }
}
</script>

<template>
  <h4>{{ msg }} - {{ value }}</h4>
  <h4 v-if="isGreatedThan10">Brojot e pogolem od 10</h4>
  <h4 v-else="!isGreatedThan10">Brojot ne e pogolem od 10</h4>
  <button v-on:click="increment">Increment</button>
</template>

<style>
</style>