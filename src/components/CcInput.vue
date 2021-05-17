<template>
  <div class="cc-input">
    <input
      v-if="propsToPass.type !== 'textarea'"
      v-bind="propsToPass"
      v-model="model"
      @blur="$emit('blur')"
      data-cy="input-field"
      ref="input"
    />
    <textarea
      v-if="propsToPass.type === 'textarea'"
      v-bind="propsToPass"
      v-model="model"
      @blur="$emit('blur')"
      data-cy="input-field"
      ref="textarea"
      @input="
        $emit('input', $event.target.value)
        autogrowTextarea($event)
      "
    />
  </div>
</template>

<script>
export default {
  name: 'CcInput',
  components: {},
  props: {
    /** v-model */
    value: { type: [String, Number], default: '' },
    /**
     * valueType: 'number' の場合、入力をnumberに変更したうえemitする。もしNaNになってしまえば、stringとしてemitする。
     * @type {'number' | undefined}
     */
    valueType: { type: [String, undefined], default: undefined },
    autofocus: { type: Boolean, default: false },
    autogrow: { type: Boolean, default: false },
  },
  mounted() {
    if (this.autofocus) {
      if (this.propsToPass.type !== 'textarea') {
        this.focusInput()
      } else {
        this.focusTextArea()
      }
    }
  },
  watch: {
    innerValue(newVal) {
      setTimeout(() => {
        this.$emit('input', newVal)
      }, 300)
    },
  },
  data() {
    return {
      innerValue: this.value,
    }
  },
  methods: {
    focusInput() {
      this.$refs.input.focus()
    },
    focusTextArea() {
      this.$refs.textArea.focus()
    },
    autogrowTextarea(e) {
      e.target.style.height = 'auto'
      e.target.style.height = `${e.target.scrollHeight}px`
    },
  },
  computed: {
    listenersWithoutInput() {
      return { ...this.$listeners, input: undefined }
    },
    model: {
      get() {
        return this.value
      },
      set(val) {
        const { propsToPass } = this
        if (propsToPass.type === 'number') {
          const valAsNumberIfNotNaN = !isNaN(Number(val)) ? Number(val) : val
          this.$emit('input', valAsNumberIfNotNaN)
          return
        }
        this.$emit('input', val)
      },
    },
    propsToPass() {
      const { $attrs, valueType } = this
      const type = $attrs.type ? $attrs.type : valueType === 'number' ? 'number' : undefined
      return { ...$attrs, type }
    },
  },
}
</script>


<style lang="sass">
.cc-input
  min-width: 0
  max-width: 100%
  display: flex
  flex-wrap: nowrap
  align-items: center
  position: relative
  textarea, input
    padding: 10px
    min-width: 0
    width: 100%
    min-height: 24px
    line-height: 24px
    outline: none
    box-shadow: none
    -webkit-appearance: none
    border: none
    z-index: 2
    position: relative
.cc-input
  min-width: 0
  max-width: 100%
  display: flex
  flex-wrap: nowrap
  align-items: center
  position: relative
  textarea, input
    padding: 10px
    min-width: 0
    width: 100%
    min-height: 24px
    line-height: 24px
    outline: none
    box-shadow: none
    -webkit-appearance: none
    border: none
    z-index: 2
    position: relative
    background: none
  textarea
    box-sizing: border-box
    min-height: 100px !important

  &::after, &::before
    border-radius: 4px
    position: absolute
    top: 0
    bottom: 0
    left: 0
    right: 0
    content: ''
    border-style: solid
    transition: border-color 300ms ease
  &::after
    border-width: 1px
    border-color: rgba(0, 0, 0, 0.24)
  &::before
    border-width: 2px
    border-color: transparent
  &:focus-within::after
    border-color: transparent
  &:focus-within::before
    border-color: purple
</style>
