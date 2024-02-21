<template>
  <header class="myHeader">
    <button>選單</button>
  </header>
  <div class="menuFrame">

  </div>
</template>

<script setup>
import { ref } from 'vue'
import data from '@/assets/menu.json'

defineProps({
  msg: String,
})

const count = ref(0)
console.log(data)
</script>

<style scoped lang="scss">
.myHeader {
  position: sticky;
  width: 100%;
  padding: 12px 16px;
  display: flex;
  justify-content: end;
  background: white;
  top: 0;
}
</style>
