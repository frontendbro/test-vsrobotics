<template>
  <label
    class="fl-checkbox"
    :class="{'error': error}"
  >
    <span
      class="fl-checkbox__custom-box"
      :class="size"
    >
      <input
        type="checkbox"
        class="fl-checkbox__input"
        :checked="checked"
        v-bind="$attrs"
        v-on="inputListeners"
      >
      <fl-icon
        v-if="checked"
        class="fl-checkbox__icon"
        name="check"
        :width="16"
        :height="16"
      />
    </span>
    <span
      class="fl-checkbox__text"
    >{{ label }}</span>
  </label>
</template>

<script>
import FlIcon from '@/components/icon/FlIcon.vue'

export default {
  name: 'FlCheckbox',
  components: { FlIcon },
  inheritAttrs: false,
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    label: {
      type: String,
      default: ''
    },

    size: {
      type: String,
      default: 'small'
    },
    checked: {
      type: Boolean,
      default: false
    },
    error: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    inputListeners() {
      const vm = this
      return {
        ...this.$listeners,
        change: (event) => {
          vm.$emit('change', event.target.checked)
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.fl-checkbox {
  display: flex;
  align-items: center;
  width: max-content;
  transition: 0.2s;
}

.fl-checkbox__input {
  position: absolute;
  z-index: -1;
  margin: 0;
  outline: none;
  opacity: 0;
}

.fl-checkbox__custom-box {
  border: 1px #606060 solid;
  border-radius: 3px;
  cursor: pointer;
  fill: #2b70f3;
}

.fl-checkbox__text {
  margin-left: 12px;
  cursor: pointer;
}

.small {
  width: 16px;
  min-width: 16px;
  height: 16px;
}

.error {
  color: red;
  fill: red;

  .fl-checkbox__custom-box {
    border: 1px red solid;
  }
}

@include media-max-md {
  .fl-checkbox {
    width: auto;
  }

  .fl-checkbox__text {
    font-size: 14px;
  }
}
</style>
