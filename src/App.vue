<template>
  <div class="main">
    <Header :activeId="getHeaderActiveId" @change-active="handleChangeActive" />
    <!-- <div class="main__block"> -->
    <!-- <Files v-if="width > 767" /> -->
    <Chat class="main__block" :activeId="getHeaderActiveId" @change-active="handleChangeActive" />
    <!-- </div> -->
    <Footer />
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Files from './components/Files.vue'
import Chat from './components/Chat.vue'
import Footer from './components/Footer.vue'

import { ref, onMounted, computed } from 'vue'

const width = ref(0)
const activeButton = ref('')

onMounted(() => {
  width.value = window.innerWidth
  window.addEventListener('resize', () => {
    width.value = window.innerWidth
  })
})

function handleChangeActive(id) {
  activeButton.value = id
}

const getHeaderActiveId = computed(() => {
  if (activeButton.value === 'mediaPlan' || activeButton.value === 'reports') {
    return activeButton.value
  }
  return ''
})
</script>

<style lang="scss">
.main {
  &__block {
    @media screen and (min-width: 768px) {
      display: flex;
      flex-direction: column;
      gap: 20px;
      padding: 0 40px;
      margin-bottom: 60px;
    }

    @media screen and (min-width: 1400px) {
      flex-direction: row;
      gap: 30px;
      padding: 0 120px;
    }
  }
}
</style>
