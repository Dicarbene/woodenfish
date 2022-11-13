<script setup lang="ts">
import { onMounted, ref } from 'vue'
import stick from '/in-projects/wooden-fish/stick.svg'
import darkStick from '/in-projects/wooden-fish/dark-stick.svg'

const { t } = useI18n()
const count = ref(0)
const brightCursorStyle = reactive({ cursor: `url(${stick}), auto` })
const darkCursorStyle = reactive({ cursor: `url(${darkStick}), auto` })
const woodenFish = ref<HTMLInputElement | null>(null)
const { x, y } = useMouse()
const addGd = () => {
  count.value++
}
const show = ref(false)
const wave = () => {
  show.value = true
  setTimeout(() => { show.value = false }, 200)
}
watchEffect(() => {
  if (count.value === 10) {
    // alert('电子木鱼也会梦见虚拟功德吗？')
    count.value = 0
  }
})
onMounted(() => {
  let cnt = 0
  woodenFish.value?.addEventListener('click', (e) => {
    const arr = [t('wooden-fish.sounds.dong'), t('wooden-fish.sounds.doo'), t('wooden-fish.sounds.der')]
    const elem = document.createElement('b')
    elem.style.zIndex = '9999'
    elem.style.position = 'absolute'
    elem.style.userSelect = 'none'
    elem.style.left = `${x.value - 15 + Math.random() * 10}px`
    elem.style.top = `${y.value - 265 + Math.random() * 5}px`
    cnt = (cnt + 1) % 3
    elem.innerText = arr[cnt]
    elem.style.fontSize = `${Math.random() * 10 + 15}px`
    let increase = 0
    let anim: NodeJS.Timeout | undefined
    const cy = y.value
    setTimeout(() => {
      anim = setInterval(() => {
        if (++increase === 150) {
          clearInterval(anim)
          woodenFish.value?.removeChild(elem)
        }
        elem.style.top = `${cy - 265 - increase}px`
        elem.style.opacity = ((150 - increase) / 120).toString()
      }, 8)
    }, 40)
    woodenFish.value?.appendChild(elem)
  })
})
</script>

<template>
  <div class="relative mt-20 mb-20 h-30 flex flex-col justify-center align-items-center">
    <p>
      {{ t('wooden-fish.benefaction') }}: {{ count }}/10
    </p>
    <div
      ref="woodenFish"
      class="mx-auto my-2 m-5 border-rd-3 text-center bt-4 hover:bg-gray-200 hover:dark:bg-gray-600 transition-colors"
      @click="addGd"
    >
      <img
        v-if="isDark" :style="darkCursorStyle" w-80 h-60 opacity-80 src="/in-projects/wooden-fish/dark-bowl.svg"
        @click="wave"
      >
      <img v-else :style="brightCursorStyle" w-80 h-60 opacity-80 src="/in-projects/wooden-fish/bowl.svg" @click="wave">
      <Transition name="bounce">
        <img
          v-if="show && isDark" :style="darkCursorStyle" w-80 h-60 opacity-80 absolute top--12
          src="/in-projects/wooden-fish/dark-bowl.svg"
        >
      </Transition>
      <Transition name="bounce">
        <img
          v-if="show && !isDark" :style="brightCursorStyle" w-80 h-60 opacity-80 absolute top--12
          src="/in-projects/wooden-fish/bowl.svg"
        >
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.bounce-enter-active {
  animation: ease-in 0.3s;
}

@keyframes ease-in {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.15);
    opacity: 0.3
  }

  100% {
    opacity: 0
  }
}
</style>
