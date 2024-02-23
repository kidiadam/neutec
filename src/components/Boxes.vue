<template>
  <div id="boxFrame">
    <div v-for="(row, idx) in boxRows" :key="idx" class="boxRow">
      <div
        v-for="(box, ind) in row.boxes"
        :key="`${idx}-${ind}`"
        class="box"
        :class="{ shined: box.shined }"
      ></div>
      <div v-if="row.ball && type === 1" class="ball ball1">0</div>
      <div v-if="row.ball && type === 1" class="ball ball2">0</div>
    </div>
    <div
      v-show="type === 2"
      v-for="(ball, idx) in ballsToCenter"
      :key="idx"
      :style="getBallPosition(ball)"
      class="ball"
    >
      0
    </div>
  </div>
</template>

<script setup>
import { reactive, watch, ref, onMounted } from "vue";

const props = defineProps({
  type: Number,
});

watch(
  () => props.type,
  (type) => {
    type === 2? startAnimationToCenter() : clearAnimationTimmer()
  }
);

const boxRows = reactive([
  {
    ball: true,
    boxes: [{ shined: false }, { shined: false }, { shined: true }],
  },
  {
    ball: false,
    boxes: [{ shined: false }, { shined: true }, { shined: false }],
  },
  {
    ball: true,
    boxes: [{ shined: false }, { shined: false }, { shined: true }],
  },
]);
const ballsToCenter = reactive([]);
const animationToCenterTimmer = ref(null);

// 使用js進行四顆球往同座標動畫繪製
const startAnimationToCenter = function () {
  const { clientWidth, clientHeight } = boxFrame;
  const endTop = clientHeight / 2
  const endLeft = clientWidth / 2
  animationToCenterTimmer.value = setInterval(() => {
    ballsToCenter.forEach(ball => {
      const { startLeft, startTop, left, top } = ball
      // 避免浮點數陷阱，到座標後重新來過
      if (left.toFixed(2) === endLeft.toFixed(2) || top.toFixed(2) === endTop.toFixed(2)) {
        ball.left = startLeft
        ball.top = startTop
      } else {
        const leftMove = (endLeft - startLeft) / 100
        const topMove = (endTop - startTop) / 100
        ball.left += leftMove
        ball.top += topMove
      }
    })
  }, 15);
};

const clearAnimationTimmer = function () {
  clearInterval(animationToCenterTimmer.value);
  animationToCenterTimmer.value = null;
};

const getBallPosition = function(ball) {
  const { left, top } = ball
  return {
    left: `${left}px`,
    top: `${top}px`
  }
}

onMounted(() => {
  const { clientWidth, clientHeight } = boxFrame;
  const top = 50 // 九宮格子高為100，取中心
  const left = clientWidth / 6 // 每排三格取第一格中心
  const bottom = clientHeight - 62 // 扣除一格高度一半及margin bottom的12px
  const right = clientWidth / 6 * 5 // 每排三格取第三格中心
  ballsToCenter[0] = { startTop: top, startLeft: left, top, left }
  ballsToCenter[1] = { startTop: top, startLeft: right, top, left: right }
  ballsToCenter[2] = { startTop: bottom, startLeft: left, top: bottom, left }
  ballsToCenter[3] = { startTop: bottom, startLeft: right, top: bottom, left: right }
});
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
#boxFrame {
  padding: 0 8px;
  width: 100%;
  position: relative;
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
    background: radial-gradient(
      circle,
      rgba(113, 81, 95, 1) 81%,
      rgba(0, 0, 0, 1) 100%
    );
    &.shined {
      animation: shine 0.5s ease infinite;
    }
  }
}
.ball {
  width: 30px;
  height: 30px;
  background-color: #a5f12b;
  border-radius: 50%;
  position: absolute;
  text-align: center;
  line-height: 30px;
  transform: translate(-50%, -50%);
  &.ball1 {
    top: 50%;
    left: calc((100% / 3 - 8px) / 2);
    animation: ball1 1.5s cubic-bezier(0.25, 0.1, 0.25, 1) infinite;
  }
  &.ball2 {
    top: 50%;
    left: calc(100% - ((100% / 3 - 8px) / 2));
    animation: ball2 1.5s cubic-bezier(0.25, 0.1, 0.25, 1) infinite;
  }
}
</style>


