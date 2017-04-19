<template>
  <div class="datePicker">
    <div class="operation">
      <a href="javascript:;"></a>
      <div class="title">
        {{months[month]}} {{year}}
      </div>
      <a href="javascript:;"></a>
    </div>
    <table cellpadding="0" cellspacing="0">
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
            <a :class="{active: selectDay === day*item, old: day<3}" href="javascript:;">{{day*item}}</a>
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
      months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec'],
      weeks: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
      date: null,
      time: null,
      month: null,
      year: null,
      selectDay: null
    }
  },
  created () {
    this.year = new Date().getFullYear()
    this.month = new Date().getMonth()
    this.selectDay = new Date().getDay()
  }
}
</script>
<style scoped>
.datePicker{
  position: absolute;
}
.operation {
  display: flex;
  box-sizing: border-box;
  border-left: 1px solid #fff;
  border-right: 1px solid #fff;
  width: 100%;
  height: 50px;
  background-color: #333;
}
.operation a{
  flex: 50px 0 0;
  height: 50px;
  line-height: 50px;
  color: #fff;
  text-decoration: none;
  position: relative;
}
.operation a::after {
  content: '';
  display: block;
  position: absolute;
  left: 50%;
  top: 50%;
  width: 8px;
  height: 8px;
  transform-origin: center center;
}
.operation a:nth-of-type(1)::after {
  border-left: 2px solid #fff;
  border-bottom: 2px solid #fff;
  transform: translate(-50%, -50%) rotateZ(45deg);
}
.operation a:nth-of-type(2)::after {
  border-right: 2px solid #fff;
  border-bottom: 2px solid #fff;
  transform: translate(-50%, -50%) rotateZ(-45deg);
}
.operation a:hover{
  background-color: #222;
}
.title {
  flex: 1;
  color: #fff;
  height: 50px;
  line-height: 50px;
}
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
  font-weight: normal;
  border: 1px solid #fff;
}
td {
  background-color: #fff;
}
td a{
  display: block;
  height: 50px;
  line-height: 50px;
  text-decoration: none;
  color: #fff;
  border-radius: 4px;
  background-color: orange;
}
td a.old {
  color: rgba(255, 255, 255, .5);
  cursor: not-allowed;
}
td a.old:hover{
  background-color: orange;
}
td a:hover,
td a.active {
  background-color: orangered;
}
</style>
