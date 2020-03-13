<template>
  <div class="calender">
    <p>
      date:<DatePicker :date="localToday" @update="setDate" />
      <button @click="setDate(new Date())">today</button>
    </p>
    <p>
      cell w:<input
        v-model.number="cellWidth"
        type="range"
        min="15"
        max="100"
      />
      h:<input v-model.number="cellHeight" type="range" min="15" max="100" />
      gap:<input v-model.number="cellGap" type="range" min="0" max="30" />
    </p>
    <ul>
      <li
        v-for="date in dateList"
        :key="date.getTime()"
        :style="getStyle(date)"
        :class="{
          isCurrentMonth: isCurrentMonth(date),
        }"
        @click="setDate(date)"
      >
        {{ getDateLabel(date) }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue'
import DatePicker from '@/components/DatePicker.vue'

const weekMs = 86400000 * 7

export default Vue.extend({
  components: {
    DatePicker,
  },
  props: {
    today: {
      type: Date as PropType<Date>,
      default: () => new Date(),
    },
  },
  data() {
    const now = new Date()
    return {
      cellWidth: 60,
      cellHeight: 40,
      cellGap: 5,
      localToday: now,
      localTodayTime: 0,
      localTodayYYMM: '',
      firstDateOfMonth: now,
      firstDateOfCalendar: now,
    }
  },
  computed: {
    dateList(): Date[] {
      const year = this.firstDateOfCalendar.getFullYear()
      const month = this.firstDateOfCalendar.getMonth()
      const mDay = this.firstDateOfCalendar.getDate()

      const dateList = []
      for (let i = 0; i < 100; i++) {
        const date = new Date(year, month, mDay + i)
        dateList.push(date)
      }
      return dateList
    },
  },
  watch: {
    today: {
      immediate: true,
      handler(val: Date) {
        this.setDate(val)
      },
    },
  },
  methods: {
    getDateLabel(date: Date) {
      const mday = date.getDate()
      return (mday === 1 ? `${date.getMonth() + 1}/` : '') + `${mday}`
    },
    getStyle(date: Date) {
      const day = date.getDay()
      const time = date.getTime()
      const diff = time - this.firstDateOfCalendar.getTime()
      const weekDiff = Math.floor(diff / weekMs)
      const x = day * (this.cellWidth + this.cellGap)
      const y = weekDiff * (this.cellHeight + this.cellGap)

      const isToday = time === this.localTodayTime

      const style = {
        background: day === 0 ? '#fcc' : day === 6 ? '#ccf' : '#fff',
        width: `${this.cellWidth}px`,
        height: `${this.cellHeight}px`,
        position: 'absolute',
        transform: `translate(${x}px, ${y}px)`,
      }
      return isToday
        ? {
            ...style,
            transform: `${style.transform} scale(${Math.random() *
              Math.random() *
              1.5 +
              1.1}) rotate(${(Math.random() > 0.5 ? 1 : -1) * 360 +
              Math.random() * Math.random() * 40}deg)`,
            'z-index': 1,
          }
        : style
    },
    isCurrentMonth(val: Date) {
      const yymm = `${val.getFullYear()}/${val.getMonth()}`
      return this.localTodayYYMM === yymm
    },
    setDate(val: Date) {
      const year = val.getFullYear()
      const month = val.getMonth()
      const mday = val.getDate()

      const date = new Date(year, month, mday)

      this.localToday = date
      this.localTodayTime = date.getTime()
      this.localTodayYYMM = `${year}/${month}`

      this.firstDateOfMonth = new Date(year, month, 1)
      const prevMonth = new Date(year, month - 1, 1)
      this.firstDateOfCalendar = new Date(
        year,
        month - 1,
        1 - prevMonth.getDay()
      )
    },
  },
})
</script>

<style scoped lang="scss">
.calender {
  // border: 1px solid #666;
  border-radius: 10px;
  padding: 10px;
}
ul {
  padding: 0;
  // margin: 0;
  // border: 1px solid #666;
}
li {
  font-size: 12px;
  border: 1px solid #eee;
  display: flex;
  margin: 0;
  padding: 0;
  text-indent: 0;
  list-style-type: none;
  transition: transform cubic-bezier(0.175, 0.885, 0.32, 1.275) 0.3s;
  border-radius: 10%;
  opacity: 0.5;
  cursor: pointer;
  &.isCurrentMonth {
    opacity: 1;
    border: 1px solid #ccc;
  }
}
</style>
