<template>
  <div id="app">
    <component :is="layout">
      <template #router>
        <router-view />
      </template>
    </component>
  </div>
</template>

<script>
import MainLayout from '@/layouts/MainLayout.vue';

export default {
  name: 'App',
  components: { MainLayout },
  computed: {
    layout() {
      return (this.$route.meta.layout ? `${this.$route.meta.layout}-layout` : 'main-layout')
    }
  }
}
</script>

<style lang="less">
@import 'style/main';

#app {
  font-family: 'Roboto', sans-serif;
}
</style>
