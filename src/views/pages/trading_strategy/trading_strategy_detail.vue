<template lang="pug">
  #e-trading-strategy-detail
    .e-container.trading-detail
      DetailHeader(
          :summary="summary"
          :orderSumData="orderSumData"
        )
      .trading-detail-container
        DetailSide(
          :summary="summary"
        )
        DetailConternt(
          :summary="summary"
          :orderSumData="orderSumData"
        )
</template>

<script>
import DetailHeader from './trading_strategy_detail_header.vue'
import DetailSide from './trading_strategy_detail_side.vue'
import DetailConternt from './trading_strategy_detail_content.vue'
import E from '../../../utils'

export default {
  data () {
    return {
      signalId: '',
      valuation: {},
      summary: {},
      orderSumData: {}
    }
  },
  components: {
    DetailHeader,
    DetailSide,
    DetailConternt
  },
  mounted() {
    this.signalId = this.$route.params.id
    this.getSummaryData()
  },
  methods: {
    // 交易汇总
    getSummaryData() {
      return E.handleRequest(E.api().post('signal/querySignalUsers', {
        signalId: this.signalId
      }))
        .then(res => {
          this.summary = res.data.content.data[0]
            this.getOrderSumData()
        })
    },
      getOrderSumData() {
          let params = {
              userId: this.summary.userId,
              mtAccId: this.summary.mtAccId
          }
          let data = {
              params
          }
          return E.handleRequest(E.api().post('report/order/getOrderSum', data))
              .then(res => {
                  this.orderSumData = res.data.content
              })
      }
  }
}
</script>

<style lang="sass" scoped>
.trading-detail
  background-color: #f0f0f0

  &-container
    display: flex
    width: 1200px
    margin: 20px auto
</style>
