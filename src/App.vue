<script setup>
import { ref, watchEffect } from 'vue';
import { useScroll, watchDebounced } from '@vueuse/core'

const sections = ref()

const { y, directions, isScrolling } = useScroll(window)

watchEffect(() => {
  if (y.value !== -1)
    sections.value = Array.prototype.slice.call(document?.querySelectorAll('body .observed-sections'))
})

const currentIndex = ref(0)

watchDebounced(y, () => {
  if (y.value >= 0 && sections.value?.length) {
    currentIndex.value = sections.value.findIndex((e) => {
      const clientRect = e.getBoundingClientRect()
      return clientRect.top >= 0
    })
  }
}, { debounce: 50, maxWait: 100 })

watchEffect(()=>{
  console.log("alo",currentIndex.value)
})
</script>

<template>
  <div class="relative flex items-center w-full h-full justify-center">
    <div>
      <h1 class="observed-sections h-screen text-center flex items-center text-[100px]"><span>Section One</span></h1>
      <h1 class="observed-sections h-screen text-center flex items-center text-[100px]"><span>Section Two</span></h1>
      <h1 class="observed-sections h-screen text-center flex items-center text-[100px]"><span>Section One</span></h1>
      <h1 class="observed-sections h-screen text-center flex items-center text-[100px]"><span>Section One</span></h1>
    </div>
    <div class="fixed right-0 top-0 p-5 bg-black text-white">
      <h3 class="text-yellow-400!" :class="{'text-yellow-400' : currentIndex === 0}">section one</h3>
      <h3 class="text-yellow-400!" :class="{'text-yellow-400' : currentIndex === 1}">section two</h3>
      <h3 class="text-yellow-400!" :class="{'text-yellow-400' : currentIndex === 2}">section three</h3>
      <h3 class="text-yellow-400!" :class="{'text-yellow-400' : currentIndex === 3}">section four</h3>
    </div>
  </div>
</template>

<style scoped></style>
