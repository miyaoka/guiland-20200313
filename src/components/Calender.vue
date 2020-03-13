<template>
  <div class="calender">
    <p>date:<DatePicker :date="localDate" @update="onUpdate" /></p>
    <p>
      cellSize:<input v-model.number="cellWidth" type="number" /><input
        v-model.number="cellHeight"
        type="number"
      />px
    </p>
    <ul>
      <li
        v-for="date in dateList"
        :key="date.getTime()"
        :style="getStyle(date)"
        :class="{ isToday: date.getTime() === localDateTime }"
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
      localDate: now,
      localDateTime: 0,
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
        this.onUpdate(val)
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
      const diff = date.getTime() - this.firstDateOfCalendar.getTime()
      const weekDiff = Math.floor(diff / weekMs)
      const x = day * (this.cellWidth + 4)
      const y = weekDiff * (this.cellHeight + 4)
      return {
        background: day === 0 ? '#fcc' : day === 6 ? '#ccf' : '#fff',
        width: `${this.cellWidth}px`,
        height: `${this.cellHeight}px`,
        position: 'absolute',
        transform: `translate(${x}px, ${y}px)`,
      }
    },
    onUpdate(val: Date) {
      const year = val.getFullYear()
      const month = val.getMonth()
      const mday = val.getDate()

      const date = new Date(year, month, mday)

      this.localDate = date
      this.localDateTime = date.getTime()

      this.firstDateOfMonth = new Date(year, month, 1)
      this.firstDateOfCalendar = new Date(
        year,
        month,
        1 - this.firstDateOfMonth.getDay()
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
  transition: ease-in 0.2s;
  border-radius: 10%;
  &.isToday {
    border: 2px solid #f00;
    // background: #f00 !important;
  }
}
</style>
