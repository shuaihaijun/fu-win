<template lang="pug">
  #e-index
    BaseLayout
      .index-iframe
        <iframe frameborder="0" width="1200" height="900" scrolling="yes" src="https://rili-d.jin10.com/open.php?fontSize=14px&theme=primary"></iframe>
</template>

<script>
import BaseLayout from '../../layout/base_layout.vue'

export default {
  components: {
    BaseLayout
  }
}
</script>

<style lang="sass" scoped>
.index
  &-iframe
    flex-wrap: wrap
    width: 1200px
    margin: 4% auto
</style>


