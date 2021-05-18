<template>
  <div class="autogrow">
    <textarea class="textarea" name="body" id="body" @input="autogrow($event)"></textarea>
    <input @keyup="debounce" type="text" class="input" name="anotherbody" id="anotherbody" v-model="value"></input>
    <p>Value:{{outputValue}}</p>
  </div>
</template>
<script>
export default {
  name: 'AutogrowAndDebounce',
  data() {
    return {
      value: '',
      outputValue: '',
      timeout: null,
    }
  },
  methods: {
    autogrow(e) {
      e.target.style.height = 'auto'
      e.target.style.height = `${e.target.scrollHeight}px`
    },
    debounce() {
      clearTimeout(this.timeout)
      var self = this
      this.timeout = setTimeout(function () {
        console.log('searching:', self.value)
        self.outputValue = self.value
      }, 1000)
    },
  },
}
</script>
<style lang="sass" scoped>
*
  padding: 0px 200px
.textarea, .input
  box-sizing: border-box
  width: 100%
  padding: 20px
  margin: 20px
  border-radius: 8px
</style>