<template>
  <div class="hello">
    <h1>何を作ろうかな</h1>
    <ul>
      <li>カレンダー</li>
      <li><s>時計</s></li>
      <li><s>ノードエディタ</s></li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    msg: {
      type: String,
      default: '',
    },
  },
})
</script>

<style scoped lang="scss">
h1 {
  background: #ffd;
  margin: 0;
  padding: 10px;
  font-size: 20px;
}
</style>
