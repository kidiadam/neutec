<template>
  <header class="myHeader">
    <button @click="emit('changeAnimationType', 1)">預設動畫</button>
    <button @click="emit('changeAnimationType', 2)">往同座標</button>
    <button @click="menuOpened = true">menu</button>
  </header>
  <div v-show="menuOpened" class="mask" @click="menuOpened = false"></div>
  <div class="menuList" :class="{ menuOpened }">
    <select v-model="selectedOption" @change="changeMenu">
      <option value="" disabled>請選擇</option>
      <option v-for="option in selectOptions" :key="option.key" :value="option.key">{{ option.text }}</option>
    </select>
    <MenuListItem :menu="menu" ref="menuListItem" />
  </div>
</template>

<script setup>
import MenuListItem from '@/components/MenuListItem.vue'
import { ref, onMounted } from 'vue'
import data from '@/assets/menu.json'

const menuOpened = ref(false)
const menuListItem = ref(null)
const menu = data
const selectOptions = ref([])
const selectedOption = ref("")

const emit = defineEmits(['changeAnimationType']);

function flattenArray(arr) {
  const savedKey = localStorage.getItem("selectedMenu")
  arr.forEach(item => {
    if (savedKey && savedKey === item.key) selectedOption.value = savedKey
    selectOptions.value.push(item)
    const { children } = item
    if (children) flattenArray(children)
  })
}
const changeMenu = function() {
  const { value } = selectedOption
  menuListItem.value.findMenuItem(value)
  localStorage.setItem ("selectedMenu", value)
}


onMounted(() => {
  const savedKey = localStorage.getItem("selectedMenu")
  if (savedKey) {
    menuListItem.value.openMenuItem(savedKey)
    menuOpened.value = true
  }
  flattenArray(menu)
})

</script>
<style scoped lang="scss">
.myHeader {
  position: sticky;
  width: 100%;
  padding: 12px 16px;
  display: flex;
  justify-content: space-between;
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
select {
  width: 95%;
  height: 32px;
  border: 1px solid #c0c4cc;
  border-radius: .25rem;
  appearance: none;
  outline: none;
  margin: 6px auto;
  padding: 0 4px;
}
</style>
