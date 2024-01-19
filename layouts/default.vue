<script setup lang='ts'>
import { watch, computed } from 'vue'
import { useThemeStore } from '~/stores'
const themeStore = useThemeStore()

const products = ref([
  {
    title: 'У нас дешевле',
    backgroundImage: 'https://images.unsplash.com/photo-1526779259212-939e64788e3c?q=80&w=2074&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    description: 't is a long established fact that a reader will be distracted by the readable content'
  },
  {
    title: 'У нас дешевле',
    backgroundImage: 'https://images.unsplash.com/photo-1542407424724-5426773d65a5?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1887&q=80',
    description: 't is a long established fact that a reader will be distracted by the readable content'
  },
  {
    title: 'У нас дешевле',
    backgroundImage: '/img/backgraound.png',
    description: 't is a long established fact that a reader will be distracted by the readable content'
  }
])
const black = ref()
const content = ref()

onMounted(() => {
  const contentHeight = content.value.scrollHeight
  const blackHeight = black.value.scrollHeight
  if (document.body.getBoundingClientRect().width > 768) {
    if (contentHeight <= blackHeight - (blackHeight - contentHeight)) {
      black.value.style.height = '100dvh'
    } else {
      black.value.style.height = 'auto'
    }
  } else if (contentHeight < blackHeight - (blackHeight - contentHeight)) {
    black.value.style.height = '100dvh'
  } else {
    black.value.style.height = 'auto'
  }
}
)
</script>
<template>
  <div>
    <div ref="black" class="p-3 bg-black ">
      <div ref="content" class="grid align-items-stretch h-full">
        <Link rel="stylesheet" :href="themeStore.link" />
        <div class="col-12 md:col-6 xl:col-8 h-full md:h-auto ">
          <div class="relative h-full">
            <Carousel :value="products" :num-visible="1" :num-scroll="1" class="carousel h-full">
              <template #item="slotProps">
                <div class="banner-image" :style="`background-image: url('${slotProps.data.backgroundImage}');`" />
              </template>
            </Carousel>
            <div class="absolute carousel-icons flex align-items-center">
              <i class="cursor-pointer text-2xl mr-3 pi pi-instagram text-white" />
              <i class="cursor-pointer text-2xl mr-3 pi pi-github text-white" />
              <i class="cursor-pointer text-2xl mr-3 pi pi-facebook text-white" />
            </div>
            <div class="absolute carousel-content">
              <h1 class="text-white font-semibold text-5xl sm:text-7xl mt-8 md:mt-0 pt-8 md:pt-0">
                У нас дешевле
              </h1>
              <p class="text-white font-medium text-base sm:text-xl">
                t is a long established fact that a reader will be distracted by the readable content
              </p>
              <div class="flex align-items-center">
                <i class="mr-2 sm:mr-3 text-base sm:text-xl pi pi-star-fill text-white" />
                <i class="mr-2 sm:mr-3 text-base sm:text-xl pi pi-star-fill text-white" />
                <i class="mr-2 sm:mr-3 text-base sm:text-xl pi pi-star-fill text-white" />
                <i class="mr-2 sm:mr-3 text-base sm:text-xl pi pi-star-fill text-white" />
                <i class="mr-2 sm:mr-3 text-base sm:text-xl pi pi-star-fill text-gray-800" />
              </div>
            </div>
          </div>
        </div>
        <div class="col-12 md:col-6 xl:col-4">
          <slot />
        </div>
      </div>
    </div>
  </div>
</template>
