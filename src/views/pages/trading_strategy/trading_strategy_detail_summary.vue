<template lang="pug">
  .trading-detail-summary-table

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计交易手数
      .trading-detail-summary-table-value {{orderSumData.orderLots}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日均交易手数
      .trading-detail-summary-table-value {{orderSumData.orderLotsDaily}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 盈利手数
      .trading-detail-summary-table-value {{orderSumData.orderProfitLots}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 亏损手数
      .trading-detail-summary-table-value {{orderSumData.orderLossLots}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计交易笔数
      .trading-detail-summary-table-value {{orderSumData.orderCount}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日均交易笔数数
      .trading-detail-summary-table-value {{orderSumData.orderCountDaily}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 盈利笔数
      .trading-detail-summary-table-value {{orderSumData.orderProfitCount}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 亏损笔数
      .trading-detail-summary-table-value {{orderSumData.orderLossCount}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计收益
      .trading-detail-summary-table-value {{orderSumData.orderIncome}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 交易胜率
      .trading-detail-summary-table-value {{getPersent(orderSumData.orderWinRate)}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 盈利金额
      .trading-detail-summary-table-value {{orderSumData.orderProfit}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 亏损金额
      .trading-detail-summary-table-value {{orderSumData.orderLoss}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 收益率
      .trading-detail-summary-table-value {{getPersent(orderSumData.orderIncomeRate)}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 净值盈亏比
      .trading-detail-summary-table-value {{getPersent(orderSumData.orderPlRate)}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 盈利手数占比
      .trading-detail-summary-table-value {{getPersent(orderSumData.orderProfitRate)}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 亏损手数占比
      .trading-detail-summary-table-value {{getPersent(orderSumData.orderLossRate)}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 初始交易时间
      .trading-detail-summary-table-value {{getDay(orderSumData.beginDate)}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 平均持仓时间
      .trading-detail-summary-table-value {{Math.round(orderSumData.orderHoldTimeAvg / 3600)}} 小时
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 每笔平均盈利
      .trading-detail-summary-table-value {{orderSumData.orderProfitAvg}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 每笔平均亏损
      .trading-detail-summary-table-value {{orderSumData.orderLossAvg}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计交易天数
      .trading-detail-summary-table-value {{orderSumData.tradeDaySum}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 预期回报
      .trading-detail-summary-table-value {{orderSumData.orderExpectedReturn}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 单笔最大获利
      .trading-detail-summary-table-value {{orderSumData.orderProfitMax}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 单笔最大亏损
      .trading-detail-summary-table-value {{orderSumData.orderLossMax}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 杠杆
      .trading-detail-summary-table-value {{summary.leverage}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计手续费
      .trading-detail-summary-table-value {{orderSumData.orderSwap}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日连续最大获利天
      .trading-detail-summary-table-value {{orderSumData.orderProfitKeepCount}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日连亏损利天
      .trading-detail-summary-table-value {{orderSumData.orderLossKeepCount}}

    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 净值
      .trading-detail-summary-table-value {{summary.equity}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 累计隔夜息
      .trading-detail-summary-table-value {{orderSumData.orderCommission}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日连续最大获利
      .trading-detail-summary-table-value {{orderSumData.orderProfitKeep}}
    .trading-detail-summary-table-item
      .trading-detail-summary-table-label 日连续最大亏损
      .trading-detail-summary-table-value {{orderSumData.orderLossKeep}}

</template>

<script>
import E from "../../../utils";
import moment from "moment";

export default {
  props: {
    summary: {
      type: Object
    }
  },
    data() {
        return {
            orderSumData: {},
            userId: '',
            mtAccId: ''
        }
    },
    watch: {
        summary: function(v1) {
            this.userId = this.summary.userId
            this.mtAccId = this.summary.mtAccId
            this.getOrderSumData()
        }
   },
    mounted() {
        this.userId = this.summary.userId
        this.mtAccId = this.summary.mtAccId
        this.getOrderSumData()
    },
    methods: {
        getOrderSumData() {
            let params = {
                userId: this.userId,
                mtAccId: this.mtAccId
            }
            let data = {
                params
            }
            return E.handleRequest(E.api().post('report/order/getOrderSum', data))
                .then(res => {
                    this.orderSumData = res.data.content
                })
        },
        getDay: function(date) {
            return moment(date).format("YYYY-MM-DD")
        },
        getPersent: function(value) {
            if (value === null || value==='' || value === undefined) {
                return ''
            } else {
                let persent = (value * 100).toFixed(2)
                return persent + '%'
            }
        }
    }
}
</script>

<style lang="sass" scoped>
.trading-detail

  &-summary

    &-table
      display: flex
      flex-wrap: wrap
      border-top: 1px solid #e6e6e6
      border-left: 1px solid #e6e6e6

      &-item
        display: flex
        justify-content: space-between
        align-items: center
        width: 25%
        height: 40px
        padding: 5px 10px
        border-bottom: 1px solid #e6e6e6
        border-right: 1px solid #e6e6e6
        font-size: 12px

      &-label
        color: #666
</style>
