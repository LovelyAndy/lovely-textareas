<template>
  <div class="home">
    <div class="textarea">
        autofocus
        <CcInput valueType="textarea" :autofocus="true"/>
      </div>
      <div class="textarea">
        debounce
        <CcInput valueType="input" :debounce="300" />
      </div>
      <div class="textarea">
        autogrow
        <CcInput valueType="textarea" :autogrow="true"/>
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