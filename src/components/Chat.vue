<template>
  <main class="chat">
    <div ref="sidebarRef">
      <Files
        v-if="width > 768 || showSidebar"
        :show-media-plan="showMediaPlan"
        :show-reports="showReports"
        :class="{ show: showSidebar }"
        class="chat__sidebar"
      />
    </div>

    <div class="chat__block">
      <div class="chat__header">
        <h2 class="chat__title">Личный помощник</h2>
        <div class="chat__info">
          <p class="chat__name">Jim Davidson</p>
          <p class="chat__email">Jim Davidson@adaurum.ru</p>
        </div>
        <button @click="showModal = true" class="chat__button">
          <img src="../assets/images/button.svg" alt="button" />
        </button>
        <Modal v-if="showModal" @close="showModal = false" class="chat__modal" />
      </div>
      <div class="chat__main">
        <img
          v-if="messages.length === 0"
          src="../assets/images/chat-picture.png"
          alt="chat-picture"
          class="chat__picture"
        />
        <p v-if="messages.length === 0" class="chat__text">
          Это чат с администратором. Ты можешь с ним пообщаться по любому вопросу о нашем сервисе и
          узнать о ходе работы
        </p>
        <div v-else class="chat__messages">
          <span class="chat__messages-current-time">{{ currentDateTime }}</span>
          <ul class="chat__messages-list">
            <li v-for="(message, index) in messages" :key="index" class="chat__messages-item">
              <div v-if="message.author === 'Jim'" class="chat__messages-info">
                <p class="chat__messages-user">
                  {{ message.text }}
                </p>
                <div class="chat__messages-user-info">
                  <span class="chat__messages-user-name">Jim</span>
                  <span class="chat__messages-time">{{ message.time }}</span>
                </div>
              </div>
              <div
                v-else-if="message.author === 'Bot'"
                class="chat__messages-info chat__messages-info-response"
              >
                <p class="chat__messages-response">{{ message.text }}</p>
                <span class="chat__messages-time chat__message-time-response">{{
                  message.time
                }}</span>
              </div>
            </li>
          </ul>
        </div>
        <div class="chat__buttons">
          <button
            :class="{ active: activeId === 'reports' }"
            @click="handleOrderReports"
            class="chat__buttons-button button-blue"
          >
            Заказать отчет
          </button>
          <button
            :class="{ active: activeId === 'mediaPlan' }"
            @click="handleOrderMediaPlan"
            class="chat__buttons-button button-pink"
          >
            Заказать медиаплан
          </button>
        </div>
        <div class="chat__input-container">
          <textarea
            v-model="userInput"
            placeholder="Введите сообщение для администратора"
            class="chat__input"
          ></textarea>
          <div class="chat__input-icons">
            <button class="chat__input-icons-button">
              <img src="../assets/images/folder-add.svg" alt="add-folder" />
            </button>
            <button class="chat__input-icons-button">
              <img src="../assets/images/gallery.svg" alt="gallery" />
            </button>
          </div>
          <button @click="handleButtonClick('Enter')" class="chat__input-enter-button">
            <img src="../assets/images/enter.svg" alt="enter" />
            <span v-if="width > 767" class="chat__input-enter">Enter</span>
          </button>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Modal from './Modal.vue'
import Files from './Files.vue'

defineProps({
  activeId: String,
})
const emit = defineEmits(['change-active'])

const showModal = ref(false)
const width = ref(0)
const userInput = ref('')
const messages = ref([])
const currentDateTime = ref('')
const showMediaPlan = ref(false)
const showReports = ref(false)
const showSidebar = ref(false)
const sidebarRef = ref(null)

onMounted(() => {
  width.value = window.innerWidth
  window.addEventListener('resize', () => {
    width.value = window.innerWidth
  })

  currentDateTime.value = getCurrentDateTime()
  document.addEventListener('click', handleClickOutside)
})

function backendResponse(requestType) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(`Ответ на запрос ${requestType}`)
    }, 1000)
  })
}

async function handleButtonClick() {
  if (userInput.value.trim() === '') return

  messages.value.push({
    text: userInput.value,
    author: 'Jim',
    time: getCurrentTime(),
  })
  userInput.value = ''

  const response = await backendResponse('сообщение пользователя')

  messages.value.push({
    text: response,
    author: 'Bot',
    time: getCurrentTime(),
  })
}

function getCurrentTime() {
  const now = new Date()
  const hours = now.getHours().toString().padStart(2, '0')
  const minutes = now.getMinutes().toString().padStart(2, '0')
  return `${hours}:${minutes}`
}

function getCurrentDateTime() {
  const now = new Date()
  const hours = now.getHours().toString().padStart(2, '0')
  const minutes = now.getMinutes().toString().padStart(2, '0')

  return `Сегодня, ${hours}:${minutes} `
}

async function handleOrderMediaPlan() {
  await new Promise((resolve) => setTimeout(resolve, 1000))

  showMediaPlan.value = true
  emit('change-active', 'mediaPlan')
  showSidebar.value = true
}

async function handleOrderReports() {
  await new Promise((resolve) => setTimeout(resolve, 1000))

  showReports.value = true
  emit('change-active', 'reports')
  showSidebar.value = true
}

function handleClickOutside(event) {
  if (showSidebar.value && sidebarRef.value && !sidebarRef.value.contains(event.target)) {
    showSidebar.value = false
  }
}
</script>

<style lang="scss" scoped>
.chat {
  &__block {
    width: 100%;

    @media screen and (min-width: 768px) {
      box-shadow: 0px 0px 30px 0px #00000014;
      border-radius: 20px;
      padding: 28px 20px;
    }

    @media screen and (min-width: 1400px) {
      padding: 28px;
    }
  }

  &__header {
    position: relative;
    display: grid;
    grid-template-columns: 1fr auto;
    grid-template-rows: repeat(2, auto);
    align-items: center;
    padding-left: 20px;
    padding-right: 20px;
    margin-bottom: 20px;

    @media screen and (min-width: 768px) {
      grid-template-columns: repeat(3, auto);
      align-items: center;
      padding: 0;
      margin-bottom: 30px;
    }
  }

  &__title {
    font-size: 20px;
    font-weight: 600;
    line-height: 100%;
    grid-row: 1;

    @media screen and (min-width: 768px) {
      font-size: 24px;
    }
  }

  &__button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    border: none;
    // background-color: #f5f5f5;
    border-radius: 53px;
    cursor: pointer;
    grid-row: 1;

    @media screen and (min-width: 768px) {
      justify-self: end;
    }
  }

  &__info {
    display: flex;
    gap: 10px;
    font-size: 15px;
    line-height: 100%;
    grid-column: 1 / -1;

    @media screen and (min-width: 768px) {
      grid-row: 1;
      grid-column: 2;
      justify-self: end;
    }
  }

  &__name {
    font-weight: 600;
  }

  &__email {
    font-weight: 400;
    color: #525252;
  }

  &__main {
    position: relative;
    display: grid;
    grid-template-rows: 1fr auto auto;
    width: 100%;
    min-height: 640px;
    background-color: #ebebeb;
    padding-left: 36px;
    padding-right: 36px;

    @media screen and (min-width: 768px) {
      padding-left: 20px;
      padding-right: 20px;
      border-radius: 20px;
    }
  }

  &__picture {
    max-width: 130px;
    margin-top: 50px;
    margin-bottom: 20px;
    justify-self: center;

    @media screen and (min-width: 768px) {
      max-width: 180px;
      margin-top: 55px;
    }
  }

  &__text {
    display: block;
    max-width: 260px;
    font-size: 14px;
    font-weight: 500;
    line-height: 145%;
    text-align: center;
    margin-bottom: 63px;
    justify-self: center;

    @media screen and (min-width: 768px) {
      font-size: 16px;
      max-width: 350px;
    }
  }

  &__messages {
    display: flex;
    flex-direction: column;
  }

  &__messages-current-time {
    margin: 30px auto 26px;
    font-size: 15px;
    line-height: 135%;
    font-weight: 400;
    color: #525252;

    @media screen and (min-width: 768px) {
      margin: 50px auto 24px;
    }
  }

  &__messages-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 50px;
    margin-bottom: 50px;
  }

  &__messages-info {
    position: relative;
    width: fit-content;
    background-color: #fff;
    border-radius: 12px;
    padding: 16px 22px;
    font-size: 14px;
    line-height: 145%;
    font-weight: 500;

    @media screen and (min-width: 768px) {
      font-size: 16px;
    }
  }

  &__messages-info-response {
    margin-left: auto;
  }

  &__messages-user-info {
    position: absolute;
    left: 0;
    bottom: -32px;
  }

  &__messages-user-name {
    font-size: 18px;
    font-weight: 600;
    line-height: 100%;
  }

  &__messages-time {
    margin-left: 9px;
    font-size: 15px;
    font-weight: 400;
    line-height: 135%;
    color: #525252;
  }

  &__message-time-response {
    position: absolute;
    right: 0;
    bottom: -32px;
  }

  &__messages-response {
    list-style: none;
  }

  &__buttons {
    display: flex;
    flex-direction: column;
    align-items: end;
    gap: 12px;
    justify-self: end;
    align-self: end;
    margin-bottom: 20px;

    @media screen and (min-width: 768px) {
      flex-direction: row-reverse;
      margin-left: 0;
      margin-right: auto;
    }
  }

  &__buttons-button {
    padding: 12px 22px;
    border: none;
    border-radius: 21px;
    cursor: pointer;
    color: #ffffff;

    &.button-blue {
      width: 151px;
      background-color: #2676ee;

      @media screen and (min-width: 768px) {
        width: 166px;
      }
    }

    &.button-pink {
      width: 189px;
      background-color: #ee26c2;

      @media screen and (min-width: 768px) {
        width: 210px;
      }
    }
  }

  &__input-container {
    position: relative;
    display: flex;
    justify-self: center;
    align-self: end;
    width: calc(100% + 32px);
    border: 2px solid #ee26c2;
    border-radius: 20px;
    overflow: hidden;
    margin-bottom: 20px;

    @media screen and (min-width: 768px) {
      width: calc(100% + 40px);
      margin-bottom: 0;
    }
  }

  &__input {
    width: 100%;
    height: 144px;
    border: none;
    outline: none;
    padding: 24px 20px;
    font-size: 15px;
    font-weight: 400;
    line-height: 135%;
  }

  &__input-icons {
    position: absolute;
    left: 16px;
    bottom: 20px;
    display: flex;
    gap: 20px;
  }

  &__input-icons-button {
    background: transparent;
    border: none;
    padding: 0;
    cursor: pointer;
  }

  &__input-enter-button {
    position: absolute;
    right: 16px;
    bottom: 10px;
    border: none;
    background-color: transparent;
    cursor: pointer;

    @media screen and (min-width: 768px) {
      display: flex;
      flex-direction: column;
      gap: 2px;
    }
  }

  &__input-enter {
    font-size: 10px;
    font-weight: 400;
    line-height: 135%;
  }

  &__sidebar {
    transition: transform 0.5s ease;

    @media screen and (max-width: 767px) {
      position: fixed;
      min-height: 715px;
      // top: 50%;
      // left: 50%;
      z-index: 1000;
      // transform: translate(-50%, -50%) translateX(-100%);
      transform: translateX(-100%);
      // transition: transform 0.3s ease;
      &.show {
        // transform: translate(-50%, -50%) translateX(0);
        transform: translateX(0);
      }
    }

    @media screen and (min-width: 1400px) {
      height: 100%;
    }
  }
}
</style>
