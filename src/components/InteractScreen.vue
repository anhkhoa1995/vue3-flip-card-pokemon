<template>
  <div class="interact-screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(props.cardsContext.length) - 16) * 3) / 4 + 16) *
          Math.sqrt(props.cardsContext.length)
        }px`
      }"
    >
      <CardComponent
        v-for="(card, index) in props.cardsContext"
        :key="index"
        ref="cardRefs"
        :imgBackFaceUrl="`images/${card}.png`"
        :cardsContext="props.cardsContext"
        :cardsNumber="{ index, value: card }"
        @onFlip="checkRule($event)"
      ></CardComponent>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import CardComponent from './CardComponent.vue'

const rules = ref([])
const cardRefs = ref([])

const props = defineProps({
  cardsContext: {
    type: Array,
    default: () => []
  }
})

const emit = defineEmits(['onFinish'])

const checkRule = (config) => {
  if (rules.value.length == 2) return false
  rules.value.push(config.number)
  if (rules.value.length == 2 && rules.value[0].value == rules.value[1].value) {
    if (cardRefs.value[rules.value[0].index]) {
      cardRefs.value[rules.value[0].index].onDisabledMode()
    }
    if (cardRefs.value[rules.value[1].index]) {
      cardRefs.value[rules.value[1].index].onDisabledMode()
    }
    rules.value = []

    const disabledElements = document.querySelectorAll('.interact-screen .card.disabled')
    if (disabledElements && disabledElements.length == props.cardsContext.length - 2) {
      setTimeout(() => {
        emit('onFinish')
      }, 1000)
    }
  } else if (rules.value.length == 2 && rules.value[0].value != rules.value[1].value) {
    setTimeout(() => {
      if (cardRefs.value[rules.value[0].index]) {
        cardRefs.value[rules.value[0].index].onFlipBackCard()
      }
      if (cardRefs.value[rules.value[1].index]) {
        cardRefs.value[rules.value[1].index].onFlipBackCard()
      }
      rules.value = []
    }, 800)
  } else return false
}
</script>

<style lang="scss" scoped>
.interact-screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);

  .screen__inner {
    width: calc(424px);
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
  }
}
</style>
