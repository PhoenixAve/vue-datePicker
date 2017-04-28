<template>
  <div class="datePicker">
    <div class="operation">
      <a class="prevMonth" href="javascript:;" @click="prevMonth" :class="{disabled: selectMonth <= nowMonth && selectYear <= nowYear}"></a>
      <div class="title">
        {{selectYear+'年'+getMonth+'月'}}
      </div>
      <a class="nextMonth" href="javascript:;" @click="nextMonth" :class="{disabled: selectMonth + 1 === endMonth && selectYear >= endYear}"></a>
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
        <tr v-for="week in calcWeeks">
          <td v-for="date in 7">
            <a @click="changeSelect(calcDate(week, date))" :class="{active: nowDate === calcDate(week, date) && nowYear === selectYear && nowMonth === selectMonth && selectIndex === null, selected: selectIndex === calcDate(week, date), old: calcDate(week, date) < nowDate && selectYear === nowYear && selectMonth === nowMonth, not: calcDate(week, date) === '' || (endYear === selectYear && endMonth === (selectMonth + 1) && calcDate(week, date) > endDate)}" href="javascript:;">{{calcDate(week, date)}}</a>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- <div class="time">
      <span>12</span>:<span>30</span>
      <span>14</span>:<span>30</span>
    </div> -->
  </div>
</template>

<script>
const BIGMONTH = [1, 3, 5, 7, 8, 10, 12]
const SMALLMONTH = [4, 6, 9, 11]
export default {
  name: 'datePicker',
  data () {
    return {
      // months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec'],
      // weeks: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
      weeks: ['日', '一', '二', '三', '四', '五', '六'],
      date: null,
      time: null,
      selectIndex: null,
      nowMonth: null,
      nowYear: null,
      nowDate: null,
      nowDay: null,
      selectMonth: null,
      selectYear: null,
      selectDay: null,
      selectDate: null,
      // selectWeek: null,
      endYear: 2020,
      endMonth: 6,
      endDate: 20,
      firstDay: 1,
      lastDay: 31
    }
  },
  created () {
    this._init()
  },
  computed: {
    /**
     * 计算有多少周
     * @return {[type]} [description]
     */
    calcWeeks () {
      return Math.ceil((this.firstDay + this.lastDay) / 7)
    },
    getMonth () {
      return (this.selectMonth + 1) > 9 ? (this.selectMonth + 1) : '0' + (this.selectMonth + 1)
    }
  },
  methods: {
    /**
     * 初始化时间
     * @return {[type]} [description]
     */
    _init () {
      // 设置初始值为当前日期
      this.date = new Date()
      // 初始化选中的日期
      this._calcSelectTime()
      // 当前年份
      this.nowYear = this.date.getFullYear()
      // 当前月份
      this.nowMonth = this.date.getMonth()
      // 当前月份的某一天
      this.nowDate = this.date.getDate()
      // 一周中的第几天
      // this.nowDay = this.date.getDay()

      // 1号是当月第几天
      // this.firstDay = new Date(this.nowYear, this.nowMonth, 1).getDay()
      // this.lastDay = this._calcDays()
    },
    _calcSelectTime () {
      // 当前年份
      this.selectYear = this.date.getFullYear()
      // 当前月份
      this.selectMonth = this.date.getMonth()
      // 当前月份的某一天
      this.selectDate = this.date.getDate()
      // 一周中的第几天
      this.nowDay = this.date.getDay()

      // 1号是当月第几天
      this.firstDay = new Date(this.selectYear, this.selectMonth, 1).getDay()
      this.lastDay = this._calcDays()
    },
    /**
     * 计算当前选中的月份的天数
     * @return {[Number]} [当前月份的天数]
     */
    _calcDays () {
      let month = this.selectMonth + 1
      if (BIGMONTH.some((val) => val === month)) {
        return 31
      } else if (SMALLMONTH.some((val) => val === month)) {
        return 30
      } else if (this._isLeapYear(this.selectYear)) {
        return 29
      } else {
        return 28
      }
    },
    changeSelect (date) {
      if (date === '' || (this.selectYear === this.nowYear && this.selectMonth === this.nowMonth && date < this.nowDate) || (this.selectYear === this.endYear && this.selectMonth + 1 === this.endMonth && date > this.endDate) && (date < this.nowDate || date > this.lastDay)) {
        return
      }
      this.selectDate = date
      this.selectIndex = date
      console.log(this.selectYear + '-' + (this.selectMonth + 1) + '-' + this.selectDate)
    },
    /**
     * 根据星期和星期的第几天计算当天为多少号
     * @param  {[type]} week [description]
     * @param  {[type]} date [description]
     * @return {[type]}      [description]
     */
    calcDate (week, date) {
      let calcDate = date + (week - 1) * 7 - this.firstDay
      return (calcDate <= 0 || calcDate > this.lastDay) ? '' : calcDate
    },
    prevMonth () {
      if (this.selectMonth === this.nowMonth && this.selectYear === this.nowYear) {
        return
      }
      this.selectIndex = null
      this.date = new Date(this.selectYear, this.selectMonth - 1, 1)
      this._calcSelectTime()
    },
    nextMonth () {
      if (this.selectYear === this.endYear && this.selectMonth + 1 === this.endMonth) {
        return
      }
      this.selectIndex = null
      this.date = new Date(this.selectYear, this.selectMonth + 1, 1)
      this._calcSelectTime()
    },
    _isLeapYear (year) {
      return year % 4 === 0 && year % 100 !== 0 || year % 400 === 0
    }
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
  border-left: 1px solid rgba(255, 255, 255, 1);
  border-right: 1px solid rgba(255, 255, 255, 1);
  width: 100%;
  height: 40px;
  background-color: #333;
}
.operation a{
  flex: 40px 0 0;
  height: 40px;
  line-height: 40px;
  color: rgba(255, 255, 255, 1);
  text-decoration: none;
  position: relative;
}

.operation a.disabled {
  cursor: not-allowed
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
  border-left: 2px solid rgba(255, 255, 255, 1);
  border-bottom: 2px solid rgba(255, 255, 255, 1);
  transform: translate(-50%, -50%) rotateZ(45deg);
}
.operation a:nth-of-type(2)::after {
  border-right: 2px solid rgba(255, 255, 255, 1);
  border-bottom: 2px solid rgba(255, 255, 255, 1);
  transform: translate(-50%, -50%) rotateZ(-45deg);
}
.operation a:hover{
  background-color: #222;
}
.title {
  flex: 1;
  color: rgba(255, 255, 255, 1);
  height: 40px;
  line-height: 40px;
}
table {
  border-collapse: collapse;
}
tr, td {
  width: 40px;
  height: 40px;
}
tr {
  background-color: #333;
  color: rgba(255, 255, 255, 1);
}
th, td {
  font-weight: normal;
  border: 1px solid rgba(255, 255, 255, 1);
}
td {
  background-color: rgba(255, 255, 255, 1);
}
td a{
  display: block;
  width: 40px;
  height: 40px;
  line-height: 40px;
  text-decoration: none;
  color: rgba(255, 255, 255, 1);
  border-radius: 4px;
  background-color: rgba(78,209,255,0.6);
}

td a:hover,
td a.active,
td a.selected {
  background-color: rgba(78,209,255,1);
}
td a.old {
  color: rgba(255, 255, 255, 1);
  cursor: not-allowed;
  background-color: rgba(78,209,255,0.4);
}
td a.old:hover{
  background-color: rgba(78,209,255,0.4);
}
td a.not {
  color: rgba(255, 255, 255, .5);
  cursor: not-allowed;
  background-color: rgba(78,209,255,0.4);
}
td a.not:hover{
  background-color: rgba(78,209,255,0.4);
}
</style>
