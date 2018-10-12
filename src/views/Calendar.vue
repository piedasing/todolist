<template>
  <div class="calendar">
    <div class="calendar-header">
      <i class="fas fa-angle-left icon" v-on:click="changeMonth(-1)"></i>
      <p>{{ year + '年 ' + month + '月' }}</p>
      <i class="fas fa-angle-right icon" v-on:click="changeMonth(1)"></i>
    </div>
    <div class="calendar-weekdays">
      <ul class="weekdays">
        <li v-for="(day, weekkey) in days" :key="weekkey">{{ day }}</li>
      </ul>
    </div>
    <div class="calendar-dates">
      <ul class="dates">
        <li v-for="(blank, key) in firstDayOfMonth" :key="`before${key}`" class="dateIsOver"></li>
        <li v-for="(date, key) in daysInMonth" :key="key">
          <p class="date">{{date}}</p>
        </li>
        <li v-for="(blank, key) in endBlank" :key="`after${key}`" class="dateIsOver"></li>
      </ul>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import moment from 'moment';

export default {
  name: 'calendar',
  data() {
    return {
      today: moment(),
      dateContext: moment(),
      days: ['日', '一', '二', '三', '四', '五', '六'],
      nowMonth: '',
    };
  },
  methods: {
    changeMonth(bol) {
      console.log(bol);
    },
  },
  computed: {
    year() {
      const t = this;
      return t.dateContext.format('Y');
    },
    month() {
      const t = this;
      let month = t.dateContext.format('MMMM');
      switch (month) {
        case 'January':
          month = 1;
          break;
        case 'February':
          month = 2;
          break;
        case 'March':
          month = 3;
          break;
        case 'April':
          month = 4;
          break;
        case 'May':
          month = 5;
          break;
        case 'June':
          month = 6;
          break;
        case 'July':
          month = 7;
          break;
        case 'August':
          month = 8;
          break;
        case 'September':
          month = 9;
          break;
        case 'October':
          month = 10;
          break;
        case 'November':
          month = 11;
          break;
        case 'December':
          month = 12;
          break;
        default:
          month = 1;
      }
      return month;
    },
    daysInMonth() {
      const t = this;
      return t.dateContext.daysInMonth();
    },
    currentDate() {
      const t = this;
      return t.dateContext.get('date');
    },
    firstDayOfMonth() {
      const t = this;
      const firstDay = moment(t.dateContext).subtract((t.currentDate - 1), 'days');
      return firstDay.weekday();
    },
    endBlank() { // 算每個月後面還缺幾個空格
      let blank = 35 - this.firstDayOfMonth - this.daysInMonth;
      if (blank < 0) {
        blank = 42 - this.firstDayOfMonth - this.daysInMonth;
      }
      // console.log(blank);
      return blank;
    },
    initialDate() {
      const t = this;
      return t.today.get('date');
    },
    initialMonth() {
      const t = this;
      return t.today.format('MMMM');
    },
    initialYear() {
      const t = this;
      return t.today.format('Y');
    },
  },
};
</script>

<style lang="sass" scoped>
@import 'src/assets/sass/global.sass'

.calendar
  +size(100%)
  padding: 0 10%
  .calendar-header
    +size(100%, 3rem)
    +center
    font-size: 1.5rem
    p
      margin: 0 5rem
  .calendar-weekdays
    +size(100%, 2rem)
    ul.weekdays
      +size(100%)
      +center
      li
        +size(calc(100% / 7), 100%)
        +center
  .calendar-dates
    width: 100%
    ul.dates
      width: 100%
      +center
      flex-wrap: wrap
      li
        +size(calc(100% / 7), 15vh)
        +center
        align-items: flex-start
        border-right: 1px solid #aaa
        border-bottom: 1px solid #aaa
        &:nth-child(7n + 1)
          border-left: 1px solid #aaa
        &:nth-child(1), &:nth-child(2), &:nth-child(3), &:nth-child(4),
        &:nth-child(5), &:nth-child(6), &:nth-child(7)
          border-top: 1px solid #aaa
    .date
      +size(100%, 1rem)
      +center
      background-color: #666
</style>
