<template>
  <div class="home">
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <Calender />
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'
import Calender from '@/components/Calender.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld,
    Calender,
  },
}
</script>
