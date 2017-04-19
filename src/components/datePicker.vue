<template>
  <div class="datePicker">
    <table>
      <thead>
        <tr>
          <th v-for="item in weeks">
            {{item}}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in 5">
          <td v-for="day in 7">
            <a href="javascript:;">{{day*item}}</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'datePicker',
  data () {
    return {
      weeks: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
      date: null,
      time: null
    }
  }
}
</script>
<style scoped>
table {
  border-collapse: collapse;
}
tr, td {
  width: 50px;
  height: 50px;
}
tr {
  background-color: #333;
  color: #fff;
}
th, td {
  border: 1px solid #fff;
}
td {
  background-color: orange;
}
a{
  text-decoration: none;
  color: #fff;
}
</style>
