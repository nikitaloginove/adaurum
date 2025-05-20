<template>
  <footer class="footer">
    <div class="footer__info">
      <img src="../assets/images/logo-footer.svg" alt="logo" class="footer__logo" />
      <a href="#" class="footer__text">Политика конфиденциальности</a>
    </div>
    <div class="footer__langs">
      <img src="../assets/images/global.svg" alt="global" />
      <button class="footer__langs-button button-main">RU</button>
      <button class="footer__langs-button">EN</button>
      <button class="footer__langs-button">TUR</button>
    </div>
    <div class="footer__mail">
      <img src="../assets/images/sms.svg" alt="sms" />
      <a href="mailto:pinkchicken@adaurum.ru" class="footer__mail-email">pinkchicken@adaurum.ru</a>
    </div>
    <div class="footer__social">
      <button class="footer__social-button">
        <img src="../assets/images/telegram.svg" alt="telegram" />
      </button>
      <button class="footer__social-button">
        <img src="../assets/images/whatsapp.svg" alt="whatsapp" />
      </button>
    </div>
  </footer>
</template>

<script setup></script>

<style lang="scss" scoped>
.footer {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 40px 20px;

  @media screen and (min-width: 768px) {
    display: grid;
    grid-template-columns: repeat(3, auto);
    grid-template-rows: repeat(2, auto);
    grid-row-gap: 28px;
    grid-column-gap: 0;
    padding: 32px 40px;
    background-color: #ebebeb;
  }

  @media screen and (min-width: 1400px) {
    display: flex;
    flex-direction: row;
    gap: 64px;
    padding: 28px 120px;
  }

  &__info {
    display: flex;
    flex-direction: column;
    gap: 20px;

    @media screen and (min-width: 768px) {
      grid-column: 1 / -1;
      flex-direction: row;
      align-items: baseline;
      gap: 40px;
    }

    @media screen and (min-width: 1400px) {
      width: 100%;
      justify-content: space-between;
      align-items: center;
    }
  }

  &__logo {
    max-width: 181px;

    @media screen and (min-width: 768px) {
      min-width: 209px;
    }

    @media screen and (min-width: 1400px) {
      min-width: 219px;
    }
  }

  &__text {
    font-size: 15px;
    font-weight: 500;
    line-height: 145%;
    color: #525252;
    text-decoration: none;

    @media screen and (min-width: 768px) {
      grid-column: 1 / -1;
      grid-row: 1;
    }
  }

  &__langs {
    display: flex;
    gap: 12px;
    align-items: center;

    @media screen and (min-width: 768px) {
      grid-column: 2 / 3;
    }
  }

  &__langs-button {
    background-color: transparent;
    border: none;
    color: #525252;
    cursor: pointer;
  }

  &__mail {
    display: flex;
    gap: 12px;
    align-items: center;
  }

  &__mail-email {
    font-size: 15px;
    font-weight: 500;
    line-height: 145%;
    color: #525252;
    text-decoration: none;
  }

  &__social {
    display: flex;
    gap: 12px;

    @media screen and (min-width: 768px) {
      grid-row: 2 / 3;
    }
  }

  &__social-button {
    background-color: transparent;
    border: none;
    cursor: pointer;
  }
}

.button-main {
  color: #ee26c2;
}
</style>
