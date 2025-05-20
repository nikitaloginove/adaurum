<template>
  <section class="files">
    <h2 class="files__title">Файлы</h2>
    <div v-if="!showMediaPlan && !showReports" class="files__block">
      <img src="../assets/images/files.png" alt="files" class="files__img" />
      <p class="files__text">Закажи у личного помощника медиаплан. Он появится в этом разделе</p>
    </div>
    <div v-else class="files__content">
      <div v-if="showMediaPlan" class="files__content-block">
        <div class="files__content-header">
          <span class="files__content-title">Медиапланы</span>
          <button
            @click="toggleMediaPlans"
            class="files__content-header-button"
            :class="{ rotated: isMediaPlansOpen }"
          >
            <img src="../assets/images/arrow-up.svg" alt="open-list" />
          </button>
        </div>
        <ul v-if="isMediaPlansOpen" class="files__content-list">
          <li v-for="(item, index) in mediaPlans" :key="index" class="files__content-item">
            <img src="../assets/images/folder-2.svg" alt="folder" class="files__content-img" />
            {{ item }}
            <button v-if="index === 0" class="files__content-refresh">
              <img src="../assets/images/refresh.svg" alt="refresh" />
            </button>
            <button v-else class="files__content-button">
              <img src="../assets/images/arrow-down.svg" alt="arrow-button" />
            </button>
            <div v-if="index === 0" class="files__content-process">
              <img src="../assets/images/process-icon.svg" alt="in process" />
              <p>Медиаплан в процессе составления</p>
            </div>
          </li>
          <button class="files__content-show">Показать еще</button>
        </ul>
      </div>
      <div v-if="showReports" class="files__content-block">
        <div class="files__content-header">
          <span class="files__content-title">Отчеты</span>
          <button
            @click="toggleReports"
            class="files__content-header-button"
            :class="{ rotated: isReportsOpen }"
          >
            <img src="../assets/images/arrow-up.svg" alt="open-list" />
          </button>
        </div>
        <ul v-if="isReportsOpen" class="files__content-list">
          <li v-for="(item, index) in reports" :key="index" class="files__content-item">
            <img src="../assets/images/folder-2.svg" alt="folder" class="files__content-img" />
            {{ item }}
            <button v-if="index === 0" class="files__content-refresh">
              <img src="../assets/images/refresh.svg" alt="refresh" />
            </button>
            <button v-else class="files__content-button">
              <img src="../assets/images/arrow-down.svg" alt="arrow-button" />
            </button>
            <div v-if="index === 0" class="files__content-process">
              <img src="../assets/images/process-icon.svg" alt="in process" />
              <p>Отчет формируется</p>
            </div>
          </li>
          <button class="files__content-show">Показать еще</button>
        </ul>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  showMediaPlan: Boolean,
  showReports: Boolean,
})

const isMediaPlansOpen = ref(false)
const isReportsOpen = ref(false)

const mediaPlans = [
  'Companyname 11/23',
  'Companyname 10/23',
  'Companyname 9/23',
  'Companyname 8/23',
]

const reports = ['Companyname 11/23', 'Companyname 10/23', 'Companyname 9/23']

function toggleMediaPlans() {
  isMediaPlansOpen.value = !isMediaPlansOpen.value
}

function toggleReports() {
  isReportsOpen.value = !isReportsOpen.value
}
</script>

<style lang="scss" scoped>
.files {
  border-radius: 20px;
  padding: 30px 20px;
  background-color: #fff;
  box-shadow: 0px 0px 30px 0px #00000014;
  min-width: 295px;
  display: flex;
  flex-direction: column;
  gap: 30px;

  &__title {
    font-size: 24px;
    font-weight: 600;
    line-height: 100%;
    margin-bottom: 20px;
  }

  &__block {
    display: flex;
    width: 100%;
    gap: 20px;
    justify-content: center;
    align-items: center;

    @media screen and (min-width: 1400px) {
      flex-direction: column;
      gap: 30px;
    }
  }

  &__img {
    max-width: 248px;

    @media screen and (min-width: 1400px) {
      max-width: 256px;
    }
  }

  &__text {
    text-align: center;
    font-size: 16px;
    font-weight: 500;
    line-height: 145%;
    max-width: 255px;
    text-align: start;

    @media screen and (min-width: 1400px) {
      text-align: center;
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
    gap: 30px;

    @media screen and (min-width: 768px) and (max-width: 1399px) {
      flex-direction: row;
      gap: 20px;
    }
  }

  &__content-block {
    width: 100%;
  }

  &__content-header {
    display: flex;
    justify-content: space-between;
    // width: 305px;
    align-items: center;
  }

  &__content-header-button {
    margin-right: 8px;
    background-color: transparent;
    border: none;
    cursor: pointer;
    width: 24px;
    height: 24px;
    transform: rotate(180deg);
    transition: transform 0.3s ease;

    &.rotated {
      transform: rotate(0deg);
    }
  }

  &__content-title {
    display: block;
    font-size: 18px;
    line-height: 100%;
    font-weight: 600;
  }

  &__content-list {
    position: relative;
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-top: 20px;
  }

  &__content-item {
    position: relative;
    // min-width: 314px;
    display: flex;
    align-items: center;
    background-color: #f5f5f5;
    border-radius: 4px;

    &:first-child {
      margin-bottom: 40px;
    }
  }

  &__content-img {
    padding: 9px;
  }

  &__content-refresh {
    margin-left: auto;
    border: none;
    background-color: #949494;
    width: 42px;
    height: 42px;
    border-radius: 4px;
    cursor: pointer;
  }

  &__content-button {
    margin-left: auto;
    border: none;
    background-color: #f4d4ed;
    width: 42px;
    height: 42px;
    border-radius: 4px;
    cursor: pointer;
  }

  &__content-process {
    position: absolute;
    left: 0;
    bottom: -30px;
    display: flex;
    gap: 4px;
    font-size: 12px;
    line-height: 135%;
    font-weight: 400;
    color: #525252;
  }

  &__content-show {
    margin-left: auto;
    font-size: 15px;
    font-weight: 400;
    line-height: 100%;
    color: #ee26c2;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }

  &__reports-title {
  }
}
</style>
