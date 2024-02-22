<template>
  <div v-for="item in myMenu" :key="item.key" class="menuListItem" :class="{expanded: item.expanded}" @click="clickMenu(item)">
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
}


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
