<template>
  <div class="order">
    <div class="order__price">实付金额 <b>&yen; {{caculations.price}}</b></div>
    <div class="order__btn" @click="handleShowConfirmChange(true)">提交订单</div>
  </div>
  <div class="mask" v-if="showConfirm" @click="handleShowConfirmChange(false)">
    <div class="mask__content">
      <h3 class="mask__content__title">确认离开收银台？</h3>
      <p class="mask__content__desc">请尽快完成支付，否则将被取消</p>
      <div class="mask__content__btns">
        <div class="mask__content__btn mask__content__btn--first" @click="()=>handleComfirmOrder(true)">取消订单</div>
        <div class="mask__content__btn mask__content__btn--last" @click="()=>handleComfirmOrder(false)">确认支付</div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { post } from '../../utils/request'
import { useCommonCartEffect } from '../../effects/commonCartEffect'
import { useStore } from 'vuex'

// 下单相关
const useMakeOrderEffect = (shopId, shopName, productList) => {
  const store = useStore()
  const router = useRouter()
  const handleComfirmOrder = async (isCanceled) => {
    const products = []
    for (const i in productList.value) {
      const product = productList.value[i]
      products.push({ id: parseInt(product._id, 10), num: product.count })
    }
    try {
      const result = await post('/api/order', {
        addressId: 1,
        shopId,
        shopName: shopName.value,
        isCanceled,
        products
      })
      console.log(result)
      if (result?.errno === 0) {
        store.commit('cleanCartData', shopId)
        router.push({ name: 'OrderList' })
      }
    } catch (e) {
      // showToast('请求失败')
    }
  }
  return { handleComfirmOrder }
}

// 蒙层展示相关
const useShowMaskEffect = () => {
  const showConfirm = ref(false)
  const handleShowConfirmChange = (status) => {
    showConfirm.value = status
  }
  return {
    showConfirm, handleShowConfirmChange
  }
}

export default {
  name: 'Order',
  setup() {
    const route = useRoute()
    const shopId = parseInt(route.params.id, 10)
    const { caculations, shopName, productList } = useCommonCartEffect(shopId)
    const { handleComfirmOrder } = useMakeOrderEffect(shopId, shopName, productList)
    const { showConfirm, handleShowConfirmChange } = useShowMaskEffect()
    return { caculations, handleComfirmOrder, showConfirm, handleShowConfirmChange }
  }
}
</script>

<style lang="scss" scoped>
@import "../../style/viriables.scss";
.order {
  display: flex;
  height: 0.49rem;
  line-height: 0.49rem;
  background: $bgColor;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  &__price {
    flex: 1;
    font-size: 0.14rem;
    color: $content-fontcolor;
    text-indent: 0.24rem;
  }
  &__btn {
    width: 0.98rem;
    background: #4fb0f9;
    color: $bgColor;
    font-size: 0.14rem;
    text-align: center;
  }
}
.mask {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 1;
  background: rgba(0, 0, 0, 0.5);
  &__content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 3rem;
    height: 1.56rem;
    border-radius: 0.04rem;
    background-color: #fff;
    text-align: center;
    &__title {
      font-size: 0.18rem;
      margin: 0.24rem 0 0 0;
      line-height: 0.26rem;
    }
    &__desc {
      margin: 0.08rem 0 0 0;
      font-size: 0.14rem;
      color: #666;
    }
    &__btns {
      display: flex;
      margin: 0.24rem 0.58rem;
    }
    &__btn {
      flex: 1;
      width: 0.8rem;
      line-height: 0.32rem;
      border-radius: 0.16rem;
      font-size: 0.14rem;
      &--first {
        margin-right: 0.12rem;
        color: #4fb0f9;
        border: .01rem solid #4fb0f9;
      }
      &--last {
        margin-left: 0.12rem;
        background-color: #4fb0f9;
        color: #fff;
      }
    }
  }
}
</style>
