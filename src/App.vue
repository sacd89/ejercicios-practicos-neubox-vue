<template>
  <Problem1></Problem1>
  <Problem2></Problem2>
</template>

<script>
import Problem1 from './components/Problem1.vue'
import Problem2 from './components/Problem2.vue'

export default {
  name: 'App',
  components: {
    Problem1,
    Problem2
  }
}
</script>

<style>
#app {
  padding: 50px;
}

</style>
