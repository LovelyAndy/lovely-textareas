<template>
  <div class="cc-input">
    <input
      v-if="type !== 'textarea'"
      :disabled="disabled"
      :type="type"
      :placeholder="placeholder"
      :readonly="readonly"
      :required="required"
      :autofocus="autofocus"
      v-model="model"
      data-cy="input-field"
      ref="input"
      @blur="$emit('blur')"
    />
    <textarea
      v-if="type === 'textarea'"
      :disabled="disabled"
      :placeholder="placeholder"
      :readonly="readonly"
      :required="required"
      :rows="rows"
      :autofocus="autofocus"
      data-cy="input-field"
      ref="textarea"
      v-model="model"
      @blur="$emit('blur')"
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
    /** HTML5 attribute */
    disabled: { type: String },
    /** HTML5 attribute (can also be 'textarea' in which case a `<textarea />` is rendered) */
    type: { type: String, default: 'text' },
    /** HTML5 attribute */
    placeholder: { type: String },
    /** HTML5 attribute */
    readonly: { type: Boolean },
    /** HTML5 attribute */
    required: { type: Boolean },
    /** HTML5 attribute (only for textarea) */
    rows: { type: String },
    /** v-model */
    value: { type: [String, Number], default: '' },
    autofocus: { type: Boolean, default: false },
    autogrow: { type: Boolean, default: false },
    debounce: { type: Number, default: 500 },
  },
  mounted() {},
  watch: {
    innerValue(newVal) {
      debounceInput(newVal)
      // setTimeout(() => {
      //   this.$emit('input', newVal)
      // }, 300)
    },
  },
  data() {
    return {
      innerValue: this.value,
      timeout: null,
    }
  },
  methods: {
    focusInput() {
      this.$refs.input.focus()
    },
    focusTextArea() {
      this.$refs.textarea.focus()
    },
    autogrowTextarea(e) {
      e.target.style.height = 'auto'
      e.target.style.height = `${e.target.scrollHeight}px`
    },
    debounceInput() {
      clearTimeout(this.timeout)
      var self = this
      this.timeout = setTimeout(function () {
        self.outputValue = self.value
      }, 1000)
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
        const { type } = this
        if (type === 'number') {
          const valAsNumberIfNotNaN = !isNaN(Number(val)) ? Number(val) : val
          this.$emit('input', valAsNumberIfNotNaN)
          return
        }
        this.$emit('input', val)
      },
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
