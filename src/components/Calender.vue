<template>
  <div class="calender">
    <DatePicker :date="localDate" @update="onUpdate" />
    <ul>
      <li
        v-for="date in dateList"
        :key="date.toString()"
        :style="getStyle(date)"
      >
        {{ date.getDate() }}
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
      type: Date,
      default: () => new Date(),
    },
  },
  data() {
    const now = new Date()
    return {
      cellSize: 30,
      localDate: now,
      firstDateOfMonth: now,
      firstDateOfCalendar: now,
    }
  },
  computed: {
    dateList(): Date[] {
      this.localDate
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
        this.onUpdate(val)
      },
    },
  },
  methods: {
    getStyle(date: Date) {
      const day = date.getDay()
      const diff = date.getTime() - this.firstDateOfCalendar.getTime()
      const weekDiff = Math.floor(diff / weekMs)
      const x = day * this.cellSize
      const y = weekDiff * this.cellSize
      return {
        position: 'absolute',
        transform: `translate(${x}px, ${y}px)`,
      }
    },
    onUpdate(val: Date) {
      this.localDate = val

      const year = this.localDate.getFullYear()
      const month = this.localDate.getMonth()

      this.firstDateOfMonth = new Date(year, month, 1)
      this.firstDateOfCalendar = new Date(
        year,
        month,
        1 - this.firstDateOfMonth.getDay()
      )
    },
    // getMDateLength(year: number, monthIndex: number): number {
    //   const mDateLength = mDateLengthList[monthIndex]
    //   return (
    //     mDateLength +
    //     (monthIndex === 1 && year % 4 === 0 && year % 400 !== 0 ? 1 : 0)
    //   )
    // },
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
  display: flex;
  margin: 0;
  padding: 0;
  text-indent: 0;
  list-style-type: none;
  transition: ease-in 0.3s;
}
</style>
