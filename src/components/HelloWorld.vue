<template>
  <Header @changeAnimationType="changeAnimationType" />
  <main>
    <Boxes :type="animationType" />
  </main>
</template>

<script setup>
import Header from '@/components/Header.vue'
import Boxes from '@/components/Boxes.vue'

import { ref } from 'vue'

const animationType = ref(1)
const changeAnimationType = function(value) {
  animationType.value = value
}

</script>

<style scoped lang="scss">
main {
  width: 100%;
  background: #d6d6d6;
  flex-grow: 1;
  display: flex;
  align-items: center;
}
</style>
