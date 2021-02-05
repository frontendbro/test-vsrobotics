<template>
  <div class="tabs-field">
    <FlSelect label="Купить"/>
    <FlButton title="найти" />
  </div>
</template>

<script>
import FlSelect from '@/components/FlSelect.vue'
import FlButton from '@/components/FlButton.vue';

export default {
  name: 'TabsField',
  components: { FlButton, FlSelect }
}
</script>

<style lang="scss" scoped>
.tabs-field {

}
</style>
