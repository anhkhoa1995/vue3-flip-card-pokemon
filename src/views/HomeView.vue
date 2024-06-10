<template>
  <MainScreen v-if="stepScreen == 1" @startGame="onHandleBeforeStart($event)"></MainScreen>
  <InteractScreen
    v-else-if="stepScreen == 2"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  ></InteractScreen>
  <ResultScreen
    v-else-if="stepScreen == 3"
    :timer="timer"
    @onStartAgain="stepScreen = 1"
  ></ResultScreen>
  <p class="copyright">Học code Vue3 - Khoa</p>
</template>

<script setup>
import { ref } from 'vue'
import MainScreen from '../components/MainScreen.vue'
import InteractScreen from '../components/InteractScreen.vue'
import ResultScreen from '../components/ResultScreen.vue'
import { shuffled } from '../utils/array'

const stepScreen = ref(1)
const timer = ref(0)
const settings = ref({
  totalOfBlocks: 0,
  cardsContext: [],
  startAt: null
})

const onHandleBeforeStart = (config) => {
  settings.value.totalOfBlocks = config.totalOfBlocks

  const firstCards = Array.from({ length: settings.value.totalOfBlocks / 2 }, (_, i) => i + 1)
  const secondCards = [...firstCards]
  const finalCards = [...firstCards, ...secondCards]
  settings.value.cardsContext = shuffled(shuffled(shuffled(shuffled(finalCards))))

  settings.value.startAt = new Date().getTime()

  stepScreen.value = 2
}

const onGetResult = () => {
  timer.value = new Date().getTime() - settings.value.startAt
  stepScreen.value = 3
}
</script>

<style lang="scss" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}
</style>
