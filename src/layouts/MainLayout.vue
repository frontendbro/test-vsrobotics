<template>
  <div class="main-layout">
    <header class="header">
      <div class="header-top">
        <ul class="header-top-left">
          <li><router-link to="#" class="header-top__link">Каталог активов</router-link></li>
          <li><router-link to="#" class="header-top__link">Аукционы Портала DA</router-link></li>
        </ul>
        <ul class="header-top-right">
          <li><router-link to="#" class="header-top__link">О проекте</router-link></li>
          <li><router-link to="#" class="header-top__link">Партнёрам</router-link></li>
          <li><router-link to="#" class="header-top__link">Помощь</router-link></li>
          <li><router-link to="#" class="header-top__link">Контакты</router-link></li>
          <li></li>
        </ul>
      </div>
      <div class="header-bottom">
        <ul class="header-bottom-left">
          <li><Logo/></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
        <ul class="header-bottom-right">
          <li></li>
          <li></li>
        </ul>
      </div>
    </header>
    <slot name="router"></slot>
  </div>
</template>

<script>
import Logo from '@/assets/image/logo.svg'

export default {
  name: 'MainLayout',
  components: { Logo }
}
</script>

<style lang="scss" scoped>
.main-layout {
  height: 100%;
}
.header {
  &-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: $black;
    color: $gray-03;
    height: 36px;
    font-family: 'Roboto', sans-serif;
    &__link {
      box-sizing: border-box;
      padding: 8px 12px;
      &:hover {
        background-color: rgba(82, 87, 92, 0.4);
        color: $white;
      }
    }
    &-left {
      display: flex;
      @include clear-list
    }
    &-right {
      display: flex;
      @include clear-list
    }
    a {
      color: inherit;
      text-decoration: none;
    }
  }
  &-bottom {
    min-height: 64px;
    &-left {
      display: flex;
      @include clear-list
    }
    &-right {
      display: flex;
      @include clear-list
    }
  }
}
</style>
