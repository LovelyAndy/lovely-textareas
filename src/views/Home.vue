<template>
  <div class="home">
    <div class="textarea">
        autofocus
        <CcInput type="textarea" :autofocus="true"/>
      </div>
      <div class="textarea">
        debounce
        <CcInput type="input" :debounce="debounce" />
      </div>
      <div class="textarea">
        autogrow
        <CcInput type="textarea" :autogrow="true"/>
      </div>
    </div>
  </div>
</template>

<script>
import CcInput from '../components/CcInput.vue'
export default {
  name: 'Home',
  components: {
    CcInput,
  },
}
</script>

<style lang="sass" scoped>
*
  width: 50vw
  left: 50%
.textarea
  text-align: center
</style>