<template>
  <div>
    <div class='goods_swiper_layer_bx'>
      <goods-swiper />
    </div>
    <div class='adress_layer_out_bx'>
      <flexbox class='layer_item have_under_line'>
        <flexbox-item :span='2/24'>
          <div class='position_icon_bx'><span class='icon_bx'><svg-icon iconClass='position' /></span></div>
        </flexbox-item>
         <flexbox-item class='label' :span='5/24'>营业地址</flexbox-item>
          <flexbox-item class='adress_detail' :span='17/24'>{{merchant.address}}</flexbox-item>
      </flexbox>
      <flexbox class='layer_item'>
        <flexbox-item :span='2/24'>
          <div class='position_icon_bx'><span class='icon_bx'><svg-icon iconClass='time' /></span></div>
        </flexbox-item>
         <flexbox-item class='label' :span='5/24'>营业时间</flexbox-item>
          <flexbox-item class='time_detail' :span='17/24'>{{merchant.open_time}}</flexbox-item>
      </flexbox>
    </div>
    <!-- S=备注 -->
    <div class='mark_bx'>
      <flexbox>
        <flexbox-item :span='3/24' class='mark_label'>备注</flexbox-item>
        <flexbox-item :span='21/24'>
          <input type="text" class='mark_input' v-model="mark" />
        </flexbox-item>
      </flexbox>
    </div>
    <!-- E=备注 -->
    <!-- S=支付方式 -->
    <div class='payment_way'>
      <div class="title">支付方式</div>
      <div class="payment_way_item" v-for='(item, i) in paymentList' :key='i'>
        {{item.label}}
        <span class='payment_way_check_bx'>
          <payment-check-box
          :paymentCheckId='paymentCheckId'
          :data='item'
          :checkBc='updateCheckId' />
        </span>
      </div>
    </div>
    <!-- E=支付方式 -->
    <!-- S=发票 -->
    <div class='invoice_bx' @click='writeInvoice'>
      <span>发票</span>
      <div class='right_bx'>
        <span class='tip'>{{invoiceData.title || '不需要'}}</span>
        <span class='icon_bx'><svg-icon iconClass='arrowright'></svg-icon></span>
      </div>
    </div>
    <!-- E=发票 -->
  </div>
</template>

<script>
  import { Flexbox, FlexboxItem } from 'vux'
  import GoodsSwiper from './components/goodsSwiper/goodsSwiper'
  import AdressItem from '@/components/adressItem/adressItem'
  import PaymentCheckBox from './components/paymentCheckBox/paymentCheckBox'

  import { payType } from './config'

  export default {
    name: 'home-delivery',
    components: {
      Flexbox,
      FlexboxItem,
      PaymentCheckBox,
      GoodsSwiper,
      AdressItem
    },
    props: ['returnBc'],
    data () {
      return {
        mark: '',
        paymentCheckId: null,
        merchant: {},
        paymentList: payType,
        invoiceData: {}
      }
    },
    watch: {
      mark(val) {
        this.returnBc({
          mark: val
        })
      },
      paymentCheckId(val) {
        this.returnBc({
          paymentType: val
        })
      }
    },
    mounted() {
      const merchantStr = localStorage.getItem('merchant')
      const invoiceDataStr = localStorage.getItem('invoice_data')
      if(invoiceDataStr) {
        this.invoiceData = JSON.parse(invoiceDataStr)
      }
      if(merchantStr) {
        this.merchant = JSON.parse(merchantStr)
      }
      console.log('merchant:', localStorage.getItem('merchant'))
    },
    methods: {
      updateCheckId(newId) {
        console.log(newId)
        this.paymentCheckId = newId
      },
      writeInvoice() {
        const { fullPath } = this.$route
        console.log(fullPath)
        this.$router.push({
          path: '/writeinvoice',
          query: {
            from: fullPath
          }
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import 'src/style/mixin';
  @import './index.scss';
  .adress_layer_out_bx {
    background-color: #fff;
    padding: 0 0.65rem;
    .layer_item {
      height: 2.8rem;
      &.have_under_line {
        position: relative;
        @include halfUnderLine;
      }
    }
    .position_icon_bx {
      position: relative;
      .icon_bx {
        width: 1rem;
        height: 1rem;
        @include positionMiddle;
        left: 0;
      }
    }
    .label {
      font-size: 0.75rem;
    }
    .adress_detail {
      font-size: 0.6rem;
      @include txtLineShow(2);
    }
    .time_detail {
      font-size: 0.75rem;
    }
  }
</style>
