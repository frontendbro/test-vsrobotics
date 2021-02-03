<template>
  <div class="main-layout">
    <slot name="router"></slot>
  </div>
</template>

<script>
export default {
  name: 'MainLayout'
}
</script>

<style lang="less" scoped>
.fl-empty-layout {
  height: 100%;
}
</style>
