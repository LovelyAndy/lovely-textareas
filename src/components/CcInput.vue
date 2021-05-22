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
      :debounce="debounce"
      v-model="userInput"
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
      :debounce="debounce"
      data-cy="input-field"
      ref="textarea"
      v-model="userInput"
      @blur="$emit('blur')"
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
    debounce: { type: Number, default: 1000 },
  },
  mounted() {
    if (this.autofocus) {
      focus()
    }
  },
  watch: {
    userInput(newVal) {
      if (this.type === 'textarea' && this.autogrow) {
        this.$refs.textarea.style.height = 'auto'
        this.$refs.textarea.style.height = `${this.$refs.textarea.scrollHeight}px`
      }
      clearTimeout(this.timeout)
      this.timeout = setTimeout(() => this.$emit('input', newVal), this.debounce)
    },
  },
  data() {
    return {
      timeout: null,
      userInput: null,
    }
  },
  methods: {
    focus() {
      if (this.type !== 'textarea') {
        this.$refs.input.focus()
      } else {
        this.$refs.textarea.focus()
      }
    },
  },
  computed: {
    listenersWithoutInput() {
      return { ...this.$listeners, input: undefined }
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
