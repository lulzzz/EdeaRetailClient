<<template>
  <div id="ShoppingCart">
    <!-- <h3>Shopping Cart</h3>
    <input type="text" v-model="itemCode" value="a">
    <button @click="TransactionAddItem(itemCode)">Transaction Add Item</button> -->
    <listview :transactionItems="TransactionItems"></listview>
    <div class="footer1">
      <div class="columns is-mobile removemarginbottom">
        <div class="column is-half-mobile is-half-desktop">
          <router-link to="/Coupons">
            <a class="button is-info footer-button">
                {{ $t('message.coupons') }}
              </a>
          </router-link>
        </div>
        <div class="column is-half-mobile is-half-desktop">
          <router-link to="/Points">
            <a class="button is-info footer-button">
                  {{ $t('message.implementpoint') }}           
                </a>
          </router-link>
        </div>
      </div>
      <router-link to="/Pay" class="animated fadeInLeft">
        <a class="button is-info footer-button">{{ $t('message.continuetopay') }}</a>
      </router-link>
    </div>
  </div>
</template>

<script>
import ListView from '@/components/Collections/ListView'
import {
  mapGetters,
  mapActions
} from 'vuex'
export default {
  name: 'shoppingcart',
  data() {
    return {
      itemCode: ''
    }
  },
  mounted() {
    this.$bus.$emit('headermanipulation', { title: 'לקוח כללי', showplusbutton: true, cart: 'show' })
  },
  components: {
    'listview': ListView
  },
  computed: {
    ...mapGetters('ShoppingCartModule', [
      'TemporaryTransactionNumber',
      'LeftToPay',
      'PrerequisiteTransactionData',
      'TransactionItems'
    ])
    // TemporaryTransactionNumber() {
    //   return this.$store.getters['ShoppingCartModule/TemporaryTransactionNumber']
    // },
    // LeftToPay() {
    //   return this.$store.getters['ShoppingCartModule/LeftToPay']
    // },
    // PrerequisiteTransactionData() {
    //   return this.$store.getters['ShoppingCartModule/CurrentPrerequisiteTransactionData']
    // },
    // transaction() {
    //   var temptransaction = this.$store.getters['ShoppingCartModule/TransactionItems']
    //   return temptransaction
    // }
  },
  methods: {
    ...mapActions('ShoppingCartModule', [
      'OpenTransaction',
      'TransactionAddItem',
      'TransactionRemoveItem',
      'removeItem'
    ]),
    // async TransactionAddItem(itemCode) {
    // var itemCode = this.$refs.ItemCode.value
    // this.show = true
    //  await this.$store.dispatch('ShoppingCartModule/TransactionAddItem', itemCode)
    //  this.show = false
    // },
    // TransactionRemoveItem() {
    //   var itemIndex = 0
    //   this.$store.dispatch('ShoppingCartModule/TransactionRemoveItem', itemIndex)
    // },
    removeItem(index) {
      this.log('remove item from mixin')
      this.$store.dispatch('ShoppingCartModule/TransactionRemoveItem', index)
    }
  }
}
</script>
<style scoped lang='scss'>
@import '../common/sass/base.scss';
@import '../common/sass/localization/rtl.scss';
.removemarginbottom {
  margin-bottom: 0px;
}

.footer1 {
   position:fixed;
   bottom:0;
   width:100%;
   height:100px;   /* Height of the footer */
  //  background:#6cf;
   clear: both;
}

.footer-button {  
  width: 130px;
  box-shadow: 2px 2px 2px #888888;
} 
</style>
