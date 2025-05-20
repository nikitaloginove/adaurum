<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <button class="modal-button">Обратная связь о работе сервиса</button>
      <button class="modal-button">Сменить помощника</button>
    </div>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped>
.modal-overlay {
  position: absolute;
  top: 60px;
  right: 0;
  width: 100%;
  height: 100%;

  display: flex;
  justify-content: end;
  align-items: center;
}

.modal-content {
  max-width: 332px;
  background-color: #fff;
  box-shadow: 0px 0px 30px rgba(0, 0, 0, 0.14);
  border-radius: 8px;
  z-index: 100;
  padding: 16px 32px;
  display: flex;
  flex-direction: column;
  align-items: end;
  gap: 12px;
}

.modal-button {
  background-color: transparent;
  border: none;
  cursor: pointer;

  &:hover {
    color: #ee26c2;
  }
}
</style>
