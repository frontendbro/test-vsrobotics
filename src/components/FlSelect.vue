<template>
  <div
    v-click-outside="{
      handler: close,
      events: ['mousedown'],
      isActive: isOpen
    }"
    class="fl-select-container"
    :class="{'error': error }"
    ref="select"
  >
    <div
      class="fl-select"
      :class="{ 'is-clear': clear && getValue.length > 1 }"
      @click="openSelect"
    >
      <fl-icon
        v-if="clear && getValue.length > 1"
        name="close"
        class="fl-select-icon__close"
        :width="12"
        :height="12"
        @click.stop="clearList"
      />
      <label class="fl-select-label">
        {{ label }}
        <input
          v-model="valueInput"
          type="text"
          v-bind="$attrs"
          class="fl-select-input"
          :readonly="!writeValue"
          v-on="inputListeners"
        >
      </label>
      <fl-icon
        v-if="!notIconDropdown"
        :name="getIcon"
        class="fl-select-icon__arrow"
        :width="15"
        :height="15"
      />
    </div>
    <transition name="fade">
      <div
        v-if="isOpen"
        ref="dropdown"
        class="fl-filter-select__dropdown"
        :class="{
          'top-dropdown': topDropdown,
          'width-auto': widthAuto
        }"
      >
        <transition name="disabled">
          <div
            v-if="!$slots.dropdown && !disabled"
            class="fl-filter-select__list"
          >
            <div
              v-for="(item,i) in list"
              :key="`${item.id}`"
              :class="{
                'active': checkboxes[i].data,
                'disabled': disabled
              }"
              class="fl-filter-select__list--item"
              @click="checkCheckbox(i)"
            >
              <fl-checkbox
                v-if="!hasOneValue"
                :checked="checkboxes[i].data"
                type="checkbox"
                class="fl-filter-select__list--checkbox"
                @click="checkCheckbox(i)"
              />
              {{ item.value }}
            </div>
          </div>
        </transition>
        <slot
          v-if="$slots.dropdown"
          name="dropdown"
        />
        <slot name="more"/>
      </div>
    </transition>
  </div>
</template>

<script>
import {
  disableBodyScroll,
  clearAllBodyScrollLocks
} from 'body-scroll-lock'
import FlIcon from '@/components/icon/FlIcon.vue'
import FlCheckbox from '@/components/FlCheckbox.vue'

export default {
  name: 'FlSelect',
  components: { FlCheckbox, FlIcon },
  props: {
    value: {
      type: String,
      default: ''
    },
    label: {
      type: String,
      default: ''
    },
    list: {
      type: Array,
      default: () => ([])
    },
    initList: {
      type: Array,
      default: () => ([])
    },
    clear: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    hasOneValue: {
      type: Boolean,
      default: false
    },
    iconOpen: {
      type: String,
      default: ''
    },
    iconClose: {
      type: String,
      default: ''
    },
    openUp: {
      type: Boolean,
      default: false
    },

    writeValue: {
      type: Boolean,
      default: false
    },

    notIconDropdown: {
      type: Boolean,
      default: false
    },

    error: {
      type: Boolean,
      default: false
    },

    widthAuto: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isOpen: false,
      checkboxes: [],
      topDropdown: false
    }
  },

  computed: {
    getValue() {
      return this.list.filter((_, i) => this.checkboxes[i].data)
    },

    getIcon() {
      return this.isOpen ? this.iconOpen || 'top' : this.iconClose || 'bottom'
    },
    valueInput: {
      get() { return this.value },
      set(value) { this.$emit('input', value) }
    },

    inputListeners() {
      return {
        ...this.$listeners,
        input: (event) => this.$emit('input', event.target.value)
      }
    }
  },

  watch: {
    valueInput() {
      if (this.writeValue) this.isOpen = true
    },

    isOpen(value) {
      this.$nextTick(() => {
        if (value) {
          disableBodyScroll(this.$refs.dropdown)
          this.setPosition()
        } else {
          clearAllBodyScrollLocks()
        }
      })
    },

    list() {
      this.init()
    },

    disabled() {
      this.clearList()
    },

    initList() {
      this.init()
    }
  },

  created() {
    this.init()
  },

  methods: {
    init() {
      this.list.forEach((item, i) => {
        let value = false
        if (
          this.initList.length
          && this.initList.findIndex((it) => +it.id === this.list[i].id) !== -1) {
          value = true
        }
        this.$set(this.checkboxes, i, { data: value })
      })
      this.$emit('change', this.getValue)
    },

    setPosition() {
      if (document.documentElement.clientHeight - this.$refs.select.getBoundingClientRect()
        .bottom < 260 || this.openUp) {
        this.topDropdown = true
      }
    },
    focusInput() {
      this.$refs.input.focus()
    },

    openSelect() {
      if (this.isOpen) {
        this.$emit('close')
      } else {
        this.$emit('open')
      }
      this.isOpen = !this.isOpen
    },

    close() {
      this.isOpen = false
      this.$emit('close')
    },

    checkCheckbox(i) {
      if (this.disabled) return
      if (this.hasOneValue) {
        this.init()
        this.close()
      }
      this.checkboxes[i].data = !this.checkboxes[i].data
      this.$emit('change', this.getValue)
    },

    clearList() {
      this.checkboxes.forEach((_, i) => {
        this.$set(this.checkboxes, i, { data: false })
      })
      this.$emit('change', this.getValue)
      this.$emit('clear')
    }
  }
}
</script>

<style lang="scss" scoped>

.fl-select-container {
  position: relative;
}

.fl-select {
  position: relative;
  display: flex;
  align-items: center;
  box-sizing: border-box;
  width: auto;
  height: 56px;
  padding: 0 32px 0 18px;
  overflow: hidden;
  color: #222;
  background: #f7f7f7;
  border-radius: 8px;
  cursor: pointer;
}

.fl-select-icon__arrow {
  position: absolute;
  right: 15px;
  cursor: pointer;
  fill: #606060;
}

.fl-select-icon__close {
  position: absolute;
  left: 15px;
  cursor: pointer;
  fill: #606060;
}

.fl-select-input {
  top: 0;
  width: 100%;
  height: 100%;
  text-overflow: ellipsis;
  background: #f7f7f7;
  cursor: pointer;
  outline: none;
  border: none;
}

.fl-select-input:disabled {
  color: $black;
}

::placeholder {
  color: #222;
}

::-moz-placeholder {
  color: #222;
  opacity: 1;
}

:-ms-input-placeholder {
  color: #222;
}

.fl-select-label {
  width: 100%;
  color: #606060;
  font-size: 12px;
  cursor: pointer;
}

.fl-filter-select__dropdown {
  position: absolute;
  z-index: 4;
  box-sizing: border-box;
  width: 100%;
  max-height: 260px;
  margin-top: 10px;
  overflow-x: hidden;
  overflow-y: scroll;
  background: $white;
  border-radius: 8px;
  box-shadow: $box-shadow;

  @include hidden-scrollbar;
}

.fl-filter-select__list--item {
  display: flex;
  align-items: center;
  padding: 10px;
  white-space: nowrap;
  cursor: pointer;

  &:hover {
    background: #f7f7f7;
  }
}

.fl-filter-select__list--checkbox {
  margin-right: 10px;
}

.active {
  background: #f7f7f7;
}

.disabled {
  pointer-events: none;
}

.is-clear {
  padding-left: 32px;
}

.top-dropdown {
  bottom: 66px;
}

.error {
  .fl-select {
    border: 1px #f14848 solid;
  }

  .fl-select-label {
    color: #f14848;
  }
}

.width-auto {
  width: auto;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .1s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
