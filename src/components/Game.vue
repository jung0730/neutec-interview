<template>
  <div class="container">
    <div class="grid">
      <div
        v-for="(cell, index) in cells"
        :key="index"
        class="cell"
        :class="{ flashing: flashingCells.includes(index) }"
      >
        <div v-if="index === 0" class="ball"></div>
        <div v-else-if="index === 2" class="ball"></div>
        <div v-else-if="index === 6" class="ball"></div>
        <div v-else-if="index === 8" class="ball"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cells: [0, 1, 2, 3, 4, 5, 6, 7, 8],
      flashingCells: [2, 4, 8],
    };
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #d3d3d3;
  height: 100vh;
  overflow: hidden;
}

.grid {
  display: grid;
  width: 100%;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 5%;
  grid-auto-flow: row;
}

.cell {
  height: 100px;
  border: black solid 2px;
  background: radial-gradient(circle, rgba(113, 81, 95, 1) 81%, rgba(0, 0, 0, 1) 100%);
  position: relative;
}

.flashing {
  animation: flashingAnimation 2s infinite;
}

@keyframes flashingAnimation {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
}

.ball {
  border-radius: 50%;
  width: 30px;
  height: 30px;
  background-color: #a5f12b;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  animation: moveRight 1s linear infinite;
}

.ball:before {
  content: '0';
  top: 50%;
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
}

@keyframes moveRight {
  from {
    left: 50%;
    transform: translate(-50%, -50%);
  }
  to {
    left: 150%;
  }
}
</style>
