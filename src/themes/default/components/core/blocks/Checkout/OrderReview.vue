<template>
  <div class='order-review'>
    <div class="row">
      <div class="col-xs-2 col-md-1">
        <div class="number-circle lh35 c-white brdr-circle align-center weight-700" :class="{ 'bg-black' : isActive || isFilled, 'bg-gray' : !isFilled && !isActive }">4</div>
      </div>
      <div class="col-xs-9 col-md-11">
        <div class="row">
          <div class="col-md-12" :class="{ 'c-gray' : !isFilled && !isActive }">
            <h3 class="m0">Review order</h3>
          </div>
        </div>
        <div class="row mb35 mt20" v-show="isActive">
          <div class="col-xs-12">
            <p class="h4">Please check if all data are correct</p>
            <div class="row">
              <div class="col-md-8  bg-lightgray p15 mb35 ml10">
                <label><input type="checkbox" name="checkbox" v-model="orderReview.terms" value="value">I agree for <span class="link" @click.stop="$bus.$emit('modal.toggle', 'modal-terms')">terms and conditions</span></label>
                <span class="validation-error" v-if="!$v.orderReview.terms.required">Field is required</span>
              </div>
            </div>
            <div class="row">
              <div class="col-xs-12">
                <button-full text="Place the order" @click.native="placeOrder"  :class="{ 'ripple': true, 'button-disabled' : $v.orderReview.$invalid}"/>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <modal name="modal-terms" static="terms">
      <p slot="header">Terms and conditions</p>
    </modal>
  </div>
</template>

<script>
import { coreComponent } from 'lib/themes'

import ButtonFull from 'theme/components/theme/ButtonFull.vue'
import ValidationError from 'theme/components/core/ValidationError.vue'
import Modal from 'theme/components/core/Modal.vue'
import { required } from 'vuelidate/lib/validators'

export default {
  props: ['isActive'],
  validations: {
    orderReview: {
      terms: {
        required
      }
    }
  },
  data () {
    return {
      isFilled: false,
      orderReview: {
        terms: ''
      }
    }
  },
  methods: {
    placeOrder () {
      this.$bus.$emit('checkout.placeOrder')
    }
  },
  components: {
    ButtonFull,
    ValidationError,
    Modal
  },
  mixins: [coreComponent('core/blocks/Checkout/OrderReview')]
}
</script>

<style scoped>
.link {
  cursor: pointer;
  text-decoration: underline;
}
</style>
