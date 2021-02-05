<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :width="width"
    :height="height"
    :viewBox="getDataSvg.viewBox"
    v-on="$listeners"
  >
    <path
      fill="inherit"
      fill-rule="evenodd"
      :d="getDataSvg.d"
    />
  </svg>
</template>
<script>
import mapIcon from './mapIcon'

export default {
  name: 'FlIcon',
  props: {
    name: {
      type: String,
      default: 'add'
    },
    width: {
      type: Number,
      default: 16
    },
    height: {
      type: Number,
      default: 16
    }
  },

  computed: {
    getDataSvg() {
      return mapIcon[this.name]
    }
  }
}
</script>
