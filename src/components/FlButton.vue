<script>
import FlIcon from '@/components/icon/FlIcon.vue'

export default {
  name: 'FlButton',
  functional: true,
  components: { FlIcon },
  props: {
    tag: {
      type: String,
      default: 'button'
    },
    title: {
      type: String,
      default: ''
    },
    icon: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'is-default'
    },
    heightIcon: {
      type: Number,
      default: 16
    },
    widthIcon: {
      type: Number,
      default: 16
    },
    border: {
      type: Boolean,
      default: false
    },

    fullWidth: {
      type: Boolean,
      default: false
    },

    size: {
      type: String,
      default: ''
    },

    disabled: {
      type: Boolean,
      default: false
    },

    transparent: {
      type: [Boolean, Number],
      default: false
    }
  },

  render: (h, context) => {
    function defineClassForButton() {
      let mainClass = `fl-button ${context.props.type} ${context.props.size}`
      if (context.props.border) mainClass += ' border'
      if (context.props.icon) mainClass += ' with-icon'
      if (context.props.fullWidth) mainClass += ' fill-width'
      if (context.props.disabled) mainClass += ' disabled'
      if (context.props.transparent) mainClass += ' transparent'
      if (!context.props.title) mainClass += ' not-title'
      return mainClass
    }

    function defineClassForIcon() {
      return 'fl-button__icon'
    }

    if (context.data.staticClass) {
      context.data.staticClass += ` ${defineClassForButton()}`
    } else {
      context.data.staticClass = defineClassForButton()
    }

    return h(context.props.tag, context.data, [context.props.icon
      && h(FlIcon, {
        class: defineClassForIcon(),
        props: {
          name: context.props.icon,
          width: context.props.widthIcon,
          color: 'initial',
          height: context.props.heightIcon
        }
      }),
    context.props.title && h(
      'span', {
        class: 'fl-button__title',
        domProps: {
          innerHTML: context.props.title
        }
      }
    )
    ])
  }
}
</script>

<style lang="scss" scoped>
.fl-button {
  height: 56px;
  padding: 8px 0;
  font-size: 16px;
  font-family: $main-ff;
  background: transparent;
  border: 0;
  border-radius: 5px;
  outline: 0;
  cursor: pointer;

  @include flex-center;

  &:hover {
    box-shadow: 0 2px 5px 0 rgba($black-01, 0.05);
  }
}

.is-default {
  height: auto;
  background: $light-01;
}

.is-primary {
  color: $light-01;
  background: $primary;
  fill: $light-01;

  &:hover {
    color: $light-01;
    box-shadow: 0 7px 20px 0 rgba($black-01, 0.1);
  }
}

.fl-button__title {
  text-align: center;
}

.with-icon .fl-button__title {
  margin-left: 10px;
}

.border {
  border: solid 1px $light-02;
}

.small {
  font-size: 16px;
}

.medium {
  font-size: 20px;
}

.disabled {
  background: $grey-01;
  pointer-events: none;
}

.not-title {
  align-self: flex-end;
  padding: 6px 15px;
  fill: $primary;
}

.fill-width {
  width: 100%;
}

.transparent {
  &:hover {
    box-shadow: initial;
  }
}
</style>
