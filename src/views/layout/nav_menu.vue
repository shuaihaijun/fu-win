<template lang="pug">
  #e-menu
    .menu-container
      .menu-item(
        v-for="m in menus"
        :key="m.path"
        :to="m.path"
        :class="menuActive(m.name)"
        @click="routeTo(m.name)"
      ) {{m.text}}
</template>

<script>
import _config from '../../base_config'
export default {
  data() {
    return {
      crmUrl:_config.CRM_URL,
      menus: [
          {
              text: '首页',
              path: '/index',
              name: 'index'
          },
        {
          text: '交易策略',
          path: '/trading_strategy',
          name: 'tradingStrategy'
        },
        {
          text: '关于我们',
          path: '/about',
          name: 'about'
        },
        {
          text: 'CRM',
          name: 'crm'
        }
      ],
      routeName: ''
    }
  },
  created() {
    this.routeName = this.$route.matched[0].name
    this.$router.afterEach((to, from) => {
      this.routeName = to.matched[0].name
    })
  },
  mounted() {

  },
  methods: {
    menuActive(name) {
      if (name === this.routeName) {
        return 'active'
      }
    },
    routeTo(name) {
      if(name === 'crm'){
          let newUrl = this.crmUrl
          const userInfo = window.localStorage.getItem('follow_user_info')
          if (userInfo !== null) {
              let userData = JSON.parse(userInfo)
              if (userData.token !==null) {
                  newUrl = newUrl + '?token='+userData.token
              }
          }
        window.open(newUrl)
      } else {
          this.$router.push({
              name
          })
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.menu

  &-container
    display: flex
    margin: 0 24px

  &-item
    height: 60px
    line-height: 60px
    color: #fff
    padding: 0 12px
    cursor: pointer
    font-size: 15px

    &:hover
      background-color: #696969

    &.active
      color: #409EFF
</style>



