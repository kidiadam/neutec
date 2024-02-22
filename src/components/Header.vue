<template>
  <header class="myHeader">
    <button @click="menuOpened = true">選單</button>
  </header>
    <div v-show="menuOpened" class="mask" @click="menuOpened = false"></div>
    <div class="menuList" :class="{ menuOpened }">
      <MenuListItem :menu="menu" />
    </div>
</template>

<script setup>
import MenuListItem from '@/components/MenuListItem.vue'
import { ref } from 'vue'
import data from '@/assets/menu.json'

defineProps({
  msg: String,
})

const menuOpened = ref(false)
const menu = data

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
.mask {
  height: 100vh;
  position: fixed;
  top: 0;
  right: 0;
  width: 100%;
  z-index: 98;
  background: black;
  opacity: .3;
  width: 100%;
  height: 100%;
}
.menuList {
  height: 100vh;
  position: fixed;
  top: 0;
  right: 0;
  z-index: 99;
  width: 0;
  background: black;
  top: 0;
  right: 0;
  overflow: hidden;
  transition: width .3s ease;
  padding: 12px 0;
  &.menuOpened {
    width: 200px;
  }
}
</style>
