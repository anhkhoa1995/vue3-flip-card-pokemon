<template>
  <div class="result-screen">
    <h1>✨ Chúc mừng bro! ✨</h1>
    <h3>{{ Math.round(props.timer / 1000) }} giây</h3>
    <button @click="onStartAgain">Chơi lại</button>
  </div>
</template>

<script setup>
const props = defineProps({
  timer: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['onStartAgain'])

const onStartAgain = () => {
  emit('onStartAgain')
}
</script>

<style lang="scss" scoped>
.result-screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  h1 {
    font-size: 5rem;
  }

  h3 {
    margin-top: 1.5rem;
    font-size: 3rem;
  }

  button {
    font: var(--font);
    background: transparent;
    box-shadow: none;
    border: 1px solid var(--light);
    color: var(--light);
    margin: 1rem;
    padding: 1rem 1.25rem;
    border-radius: 0.5rem;
    font-size: 1.25rem;
    cursor: pointer;
    transition: background 0.3s ease-in-out;
  }

  button:hover {
    background-color: var(--light);
    color: var(--dark);
  }
}
</style>
