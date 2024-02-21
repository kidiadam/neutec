<template>
  <Header />
  <main>
    <Boxes /> 
  </main>
</template>

<script setup>
import Header from '@/components/Header.vue'
import Boxes from '@/components/Boxes.vue'

import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)
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
