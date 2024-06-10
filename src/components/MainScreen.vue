<template>
  <div class="main-screen">
    <h1>LẬT THẺ BÀI POKEMON</h1>
    <p>Chọn một kiểu để chơi</p>
    <div class="modes">
      <button @click="btnStart(16)">
        <span>4x4</span>
        <span>Mầm non</span>
      </button>
      <button @click="btnStart(36)">
        <span>6x6</span>
        <span>Mẫu giáo</span>
      </button>
      <button @click="btnStart(64)">
        <span>8x8</span>
        <span>Tiểu học</span>
      </button>
      <!-- <button @click="btnStart(100)">
        <span>10x10</span>
        <span>Trung học</span>
      </button> -->
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['startGame'])

const btnStart = (value) => {
  emit('startGame', { totalOfBlocks: value })
}
</script>

<style lang="scss" scoped>
.main-screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);

  h1 {
    font-size: 4.5rem;
    text-transform: uppercase;
  }

  p {
    font-size: 2rem;
  }

  .modes {
    display: flex;
    margin-top: 2rem;

    button {
      font: var(--font);
      width: 150px;
      height: 150px;
      background: transparent;
      box-shadow: none;
      border: 1px solid var(--light);
      color: var(--light);
      display: flex;
      flex-direction: column;
      border-radius: 1rem;
      margin: 0 1rem;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: background 0.3s ease-in-out;
    }

    button:hover {
      background-color: var(--light);
      color: var(--dark);
    }

    button span:first-child {
      font-size: 2rem;
    }

    button span:last-child {
      display: block;
      font-size: 1.25rem;
      margin-top: 0.5rem;
    }
  }
}
</style>
