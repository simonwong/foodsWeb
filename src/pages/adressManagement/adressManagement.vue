<template>
  <div class='user_agreement'>
    <top-back type='radis' heightNum='2rem' paddingBtm='1rem'>
      <div class='write_invoice_back_bx'>
        地址管理
      </div>
    </top-back>
    <div class='goods_inventory_detail_out_bx'>
      <cube-scroll
      ref='scroll'
      :options='scrollOptions'>
        <div class='address_item' v-for='item in addressData' :key='item.id'><adress-item :dataItem='item' :fromData='fromData' /></div>
        <div class='add_adress_bt' @click='addAddress'>
          <base-button height='2rem' label='+ 新增收获地址' isRadius=true />
        </div>
      </cube-scroll>
    </div>
  </div>
</template>

<script>
  import TopBack from '@/components/topBack/topBack'
  import AdressItem from '@/components/adressItem/adressItem'
  import BaseButton from '@/components/baseButton/baseButton'
  import { getAccountBase } from '@/service/getData'

  export default {
    name: 'adress-management',
    components: {
      TopBack,
      AdressItem,
      BaseButton
    },
    data() {
      return {
        fromData: {},
        addressData: [],
        scrollOptions: {
          pullDownRefresh: false,
          pullUpLoad: false,
          scrollbar: true
        },
      }
    },
    mounted() {
      console.log(this.$route)
      const { query: { from } } = this.$route
      this.fromData = {
        path: from,
      }
      this.fetchAddressData()
    },
    methods: {
      fetchAddressData () {
        getAccountBase({
          t: 'address.list'
        }).then(res => {
          if(res && res.errcode === 0) {
            this.addressData = res.data
          }
        })
      },
      addAddress() {
        const { fullPath } = this.$route
        console.log(this.$route)
        this.$router.push({
          path: '/editadress',
          query: {
            from: fullPath
          }
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import 'src/style/mixin.scss';
  .write_invoice_back_bx {
      font-size: 0.9rem;
      width: 15rem;
      height: 2rem;
      line-height: 2rem;
      text-align: center;
    }
    .goods_inventory_detail_out_bx {
      height: 90vh;
    }
    .address_item {
      padding-bottom: 0.25rem;
    }
    .add_adress_bt {
      padding: 1.5rem 1.5rem 0;
    }
</style>
