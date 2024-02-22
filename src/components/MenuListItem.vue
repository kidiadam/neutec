<template>
  <div v-for="item in myMenu" :key="item.key" class="menuListItem" :class="{expanded: item.expanded}" @click.stop="clickMenu(item)">
    <div class="menuText" :class="{active: item.expanded}">{{ item.text }}</div>
    <div v-if="item.expanded && item.children">
      <MenuListItem :menu="item.children" />
    </div>
  </div>
</template>

<script setup>
import MenuListItem from '@/components/MenuListItem.vue'
import { reactive } from 'vue'


const props = defineProps({
  menu: Array,
})
const myMenu = reactive(props.menu)

function clickMenu(item) {
  const { key } = item
  const toClosedItem = myMenu.find(el => el.expanded && el.key !== key)
  if (toClosedItem) toClosedItem.expanded = false
  item.expanded = true
  localStorage.setItem ("selectedMenu", key)
}

function findItemWithEach(value, list) {
  let isValid = false
  list.forEach(item => {
    const { key, children } = item
    if (key === value || findItemWithEach(value, children || [])) {
      item.expanded = true
      isValid = true
    } else {
      item.expanded = false
    }
  })
  return isValid
}
// select選單觸發的menu開啟行為
function findMenuItem(value) {
  // 若使用for迴圈的中斷特性，原先排序較後面已被開啟的選單就會沒關閉到
  findItemWithEach(value, myMenu)
}

function findItemWithFor(value, list) {
  for (let item of list) {
    const { key, children } = item
    if (key === value || findItemWithFor(value, children || [])) {
      item.expanded = true
      return true
    }
  }
  return false
}
function openMenuItem(value) {
  // 根據localstorage的值開啟的不會有需要找到舊有的問題 直接用for
  findItemWithFor(value, myMenu)
}

defineExpose({
  findMenuItem,
  openMenuItem
});

</script>

<style scoped lang="scss">
.menuListItem {
  color: white;
  padding-left: 12px;
  text-align: left;
  cursor: pointer;
  &.expanded {
    background: gray;
  }
}
.menuText {
  margin-bottom: 12px;
  &.active {
    color: yellow;
  }
}
</style>
