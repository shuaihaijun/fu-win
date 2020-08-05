<template lang="pug">
  #e-logo
    router-link(
      to="/"
    )
      img.logo(
        :src="getLogoUrl"
        :style='logoStyle'
      )
</template>

<script>
import _config from "../../base_config";

export default {
  props: {
    height: {
      type: Number,
      default: 32
    }
  },
    data() {
      return {
          logoUrl: '/upload/image/logo/fengxian.svg',
          baseUrl: _config.BASE_URL,
      }
    },
  computed: {
    logoStyle() {
      return {
        height: `${this.height}px`
      }
    },
    getLogoUrl(){
        const storage = window.localStorage
        const projInfo = storage.getItem('projInfo')
        if (projInfo !== undefined && projInfo !== null) {
            this.projInfo = JSON.parse(projInfo)
            this.logoUrl = this.projInfo.projLogo
        }
        return this.baseUrl+this.logoUrl
    }
  }
}
</script>

<style lang="sass" scoped>
.logo
  display: block
</style>
