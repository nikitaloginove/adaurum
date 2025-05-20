<template>
  <header class="header">
    <img src="../assets/images/logo.svg" alt="logo" class="header__logo" />
    <div class="header__user-block">
      <div class="header__user-block-content">
        <div class="header__user-block-wrapper">
          <img src="../assets/images/convert-card.svg" alt="cart" class="header__user-block-icon" />
        </div>
        <p v-if="width > 767" class="header__user-block-text">5%</p>
      </div>
      <div class="header__user-block-content">
        <div class="header__user-block-wrapper">
          <img src="../assets/images/user.svg" alt="user" class="header__user-block-icon" />
        </div>
        <p class="header__user-block-text">Username@adaurum.ru</p>
        <p v-if="width < 768" class="header__user-block-percents">5% выручка</p>
      </div>

      <div class="header__user-block-buttons">
        <div class="header__user-block-buttons-wrapper">
          <img src="../assets/images/setting.svg" alt="settings" />
        </div>

        <div class="header__user-block-buttons-wrapper">
          <img src="../assets/images/logout.svg" alt="logout" />
        </div>
      </div>
    </div>
    <div v-if="width < 768" class="header__buttons">
      <button
        :class="[
          'header__buttons-button',
          { 'active-group': isAnyButtonActive },
          { 'active-mediaPlan': activeId === 'mediaPlan' },
        ]"
        @click="handleButtonClick('mediaPlan')"
      >
        Медиапланы
      </button>
      <button
        :class="[
          'header__buttons-button',
          { 'active-group': isAnyButtonActive },
          { 'active-reports': activeId === 'reports' },
        ]"
        @click="handleButtonClick('reports')"
      >
        Отчеты
      </button>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const props = defineProps({
  activeId: String,
})

const width = ref(0)
const emit = defineEmits(['change-active'])

function handleButtonClick(id) {
  emit('change-active', id)
}

onMounted(() => {
  width.value = window.innerWidth
  window.addEventListener('resize', () => {
    width.value = window.innerWidth
  })
})

const isAnyButtonActive = computed(() => {
  return props.activeId === 'mediaPlan' || props.activeId === 'reports'
})
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  padding: 28px 20px;
  flex-direction: column;

  @media screen and (min-width: 768px) {
    padding: 32px 40px;
    flex-direction: row;
  }

  @media screen and (min-width: 1400px) {
    padding: 32px 120px;
  }

  &__logo {
    max-width: 181px;
    margin-bottom: 20px;

    @media screen and (min-width: 768px) {
      max-width: 209px;
    }

    @media screen and (min-width: 1400px) {
      max-width: 219px;
    }
  }

  &__user-block {
    display: flex;
    align-items: center;
    margin-bottom: 20px;

    @media screen and (min-width: 768px) {
      gap: 20px;
    }

    @media screen and (min-width: 1400px) {
      gap: 26px;
    }
  }

  &__user-block-content {
    display: flex;
    flex-direction: column;
    gap: 4px;

    @media screen and (min-width: 768px) {
      flex-direction: row;
      align-items: center;
      gap: 8px;
    }
  }

  &__user-block-wrapper {
    display: none;
    @media screen and (min-width: 768px) {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background-color: #ee26c2;
      z-index: -1;
    }
  }

  &__user-block-icon {
    display: none;
    @media screen and (min-width: 768px) {
      position: relative;
      display: block;
      width: 24px;
      height: 24px;
    }
  }

  &__user-block-text {
    font-weight: 600;
    font-size: 14px;
    line-height: 100%;

    @media screen and (min-width: 768px) {
      font-size: 16px;
    }

    @media screen and (min-width: 1400px) {
      font-size: 18px;
    }
  }

  &__user-block-percents {
    font-size: 12px;
    font-weight: 500;
    line-height: 100%;
    color: #797979;
  }

  &__user-block-buttons {
    display: flex;
    gap: 12px;
    margin-left: auto;
  }

  &__user-block-buttons-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: #f5f5f5;

    @media screen and (min-width: 768px) {
      background-color: transparent;
    }

    @media screen and (min-width: 1400px) {
      background-color: #ffffff;
    }
  }

  &__buttons {
    display: flex;
    gap: 12px;
  }

  &__buttons-button {
    padding: 12px;
    font-size: 14px;
    font-weight: 600;
    line-height: 100%;
    background-color: #cbcbcb;
    border-radius: 4px;
    border: none;

    &.active-group {
      background-color: #fff;
      display: flex;
      gap: 10px;
      align-items: end;

      &:first-child {
        border: 1px solid #ee26c2;
      }

      &:last-child {
        border: 1px solid #2676ee;
      }
    }

    &.active-mediaPlan::after,
    &.active-reports::after {
      content: '';
      display: block;
      width: 16px;
      height: 16px;
      background-image: url('../assets/images/checked.svg');
    }
  }
}
</style>
