<template>
  <div class="container home">
    <div class="row">
      <div class="col">
        <div class="home__title">Реализация непрофильных и залоговых активов</div>
        <TabsField />
      </div>
    </div>
  </div>
</template>

<script>
import TabsField from '@/components/TabsField.vue';

export default {
  name: 'Home',
  components: { TabsField }
};
</script>

<style lang="scss" scoped>
.home {
  &__title {
    color: $white;
  }
}
</style>
