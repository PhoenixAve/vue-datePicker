<template>
  <div class="hello">
    <input type="text">
    <datePicker></datePicker>
  </div>
</template>

<script>
import datePicker from '@/components/datePicker'
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components: {
    datePicker
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  width: 300px;
  height: 24px;
}
</style>
