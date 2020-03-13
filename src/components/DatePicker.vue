<template>
  <div>
    <input :value="localDate" type="date" @input="onChange" />
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue'

export default Vue.extend({
  props: {
    date: {
      type: Date,
      default: () => new Date(),
    },
  },
  data() {
    return {
      localDate: '',
    }
  },
  watch: {
    date: {
      immediate: true,
      handler(val: Date) {
        this.localDate = val
          .toLocaleDateString('ja', {
            year: 'numeric',
            month: '2-digit',
            day: '2-digit',
          })
          .replace(/\//g, '-')
      },
    },
  },

  methods: {
    onChange(e: any) {
      const val = e.target?.value
      if (!val) return
      this.localDate = val
      this.$emit('update', new Date(this.localDate))
    },
  },
})
</script>

<style scoped lang="scss"></style>
