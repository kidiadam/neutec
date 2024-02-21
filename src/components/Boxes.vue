<template>
  <div class="boxFrame">
    <div v-for="row, idx in boxRows" :key="idx" class="boxRow">
      <div v-for="box, ind in row.boxes" :key="`${idx}-${ind}`" class="box" :class="{shined: box.shined}"></div>
      <div v-if="row.ball" class="ball ball1">0</div>
      <div v-if="row.ball" class="ball ball2">0</div>
    </div>
  </div>
</template>

<script setup>

import { reactive } from 'vue'

const boxRows = reactive([
  {
    ball: true,
    boxes: [
      { shined: false },
      { shined: false },
      { shined: true },
    ]
  },
  {
    ball: false,
    boxes: [
      { shined: false },
      { shined: true },
      { shined: false },
    ]
  },
  {
    ball: true,
    boxes: [
      { shined: false },
      { shined: false },
      { shined: true },
    ]
  }
])
</script>

<style lang="scss">
@keyframes shine {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
  100% {
    opacity: 1;
  }
}
@keyframes ball1 {
  0% {
    left: calc((100% / 3 - 8px) / 2);
  }
  100% {
    left: calc(100% - ((100% / 3 - 8px) / 2));
  }
}
@keyframes ball2 {
  0% {
    left: calc(100% - ((100% / 3 - 8px) / 2));
  }
  100% {
    left: 150%;
  }
}
.boxFrame {
  padding: 0 8px;
  width: 100%;
}
.boxRow {
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  position: relative;
  overflow: hidden;
  .box {
    height: 100px;
    width: calc(100% / 3 - 8px);
    background: red;
    border: black solid 2px;
    background: radial-gradient(circle, rgba(113,81,95,1) 81%, rgba(0,0,0,1) 100%);
    &.shined {
      animation: shine .5s ease infinite;
    }
  }
}
.ball {
  width: 30px;
  height: 30px;
  background-color: #A5F12B;
  border-radius: 50%;
  position: absolute;
  text-align: center;
  line-height: 30px;
  top: 50%;
  transform: translate(-50%, -50%);
  &.ball1 {
    left: calc((100% / 3 - 8px) / 2);
    animation: ball1 1.5s cubic-bezier(0.25,0.1,0.25,1) infinite;
  }
  &.ball2 {
    left: calc(100% - ((100% / 3 - 8px) / 2));
    animation: ball2 1.5s cubic-bezier(0.25,0.1,0.25,1) infinite;

  }
}
</style>


