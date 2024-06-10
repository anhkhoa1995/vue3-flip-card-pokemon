<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4}px`,
      perspective: `${((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2}px`
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard(props.cardsNumber)"
    >
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content" :style="{ backgroundImage: `url(${urlImage})` }"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  imgBackFaceUrl: {
    type: String,
    required: true
  },
  cardsNumber: {
    type: [Array, String, Number, Object]
  },
  cardsContext: {
    type: Array,
    default: function () {
      return []
    }
  }
})

const emit = defineEmits(['onFlip'])

const urlImage = computed(() => './' + props.imgBackFaceUrl)

const isFlipped = ref(false)
const isDisabled = ref(false)

const onToggleFlipCard = (number) => {
  if (isDisabled.value) return false
  isFlipped.value = !isFlipped.value
  if (isFlipped.value) emit('onFlip', { number: number })
}

const onFlipBackCard = () => {
  isFlipped.value = false
}

const onDisabledMode = () => {
  isDisabled.value = true
}

defineExpose({ onFlipBackCard, onDisabledMode })
</script>

<style lang="scss" scoped>
.card {
  display: inline-block;
  margin: 0 1rem 1rem 0;
  width: 90px;
  height: 120px;

  .card__inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
  }

  .is-flipped {
    transform: rotateY(-180deg);
  }

  .card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  }

  .card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);

    .card__content {
      background-size: contain;
      background-position: center;
      background-repeat: no-repeat;
      height: 100%;
      width: 100%;
    }
  }

  .card__face--front {
    .card__content {
      background-image: url('./images/icon_back.png');
      background-repeat: no-repeat;
      background-position: center;
      background-size: 40px 40px;
      height: 100%;
      width: 100%;
    }
  }
}

.disabled {
  .card__inner {
    cursor: default;
  }
}
</style>
