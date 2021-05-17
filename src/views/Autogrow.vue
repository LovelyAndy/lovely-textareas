<template>
  <div class="autogrow">
    <textarea class="textarea" name="body" id="body" @input="autogrow($event)"></textarea>
    <textarea class="textarea" name="anotherbody" id="anotherbody"></textarea>
  </div>
</template>
<script>
export default {
  name: 'Autogrow',
  methods: {
    autogrow(e) {
      e.target.style.height = 'auto'
      e.target.style.height = `${e.target.scrollHeight}px`
    },
  },
}
</script>
<style lang="sass" scoped>
.textarea
  box-sizing: border-box
  width: 100%
  padding: 20px
  border-radius: 8px
</style>