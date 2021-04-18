<template>
  <div class="top">
    <div class="top__header">
      <div class="iconfont top__header__back" @click="handleBackClick">&#xe6f2;</div>
      确认订单
    </div>
    <div class="top__receviver">
      <div class="top__receviver__title">收货地址</div>
      <div class="top__receviver__address">xxxxxxxxxxxxxxxxxxxxxxxx</div>
      <div class="top__receviver__info">
        <span class="top__receviver__info__name">wwj</span>
        <span class="top__receviver__info__name">18818181818</span>
      </div>
      <div class="iconfont top__receviver__icon">&#xe6f2;</div>
    </div>
  </div>
</template>

<script>
import { useRouter } from 'vue-router'
export default {
  name: 'TopArae',
  setup() {
    const router = useRouter()
    const handleBackClick = () => {
      router.back()
    }
    return { handleBackClick }
  }
}
</script>

<style lang="scss" scoped>
@import "../../style/viriables.scss";
.top {
  position: relative;
  height: 1.96rem;
  background-size: 100% 1.59rem;
  background-image: linear-gradient(0deg, rgba(0, 145, 255, 0) 4%, #0091ff 50%);
  background-repeat: no-repeat;
  &__header {
    position: relative;
    padding-top: 0.2rem;
    line-height: 0.24rem;
    color: $bgColor;
    text-align: center;
    font-size: 0.16rem;
    &__back {
      position: absolute;
      left: 0.18rem;
      font-size: 0.22rem;
    }
  }
  &__receviver {
    position: absolute;
    left: 0.18rem;
    right: 0.18rem;
    bottom: 0;
    height: 1.1rem;
    background: $bgColor;
    border-radius: 0.04rem;
    &__title {
      line-height: 0.22rem;
      padding: 0.16rem 0 0.14rem 0.16rem;
      font-size: 0.16rem;
      color: $content-fontcolor;
    }
    &__address {
      padding: 0 0.4rem 0 0.16rem;
      color: $content-fontcolor;
      font-size: 0.14rem;
      line-height: 0.2rem;
    }
    &__info {
      padding: 0.06rem 0 0 0.16rem;
      &__name {
        margin-right: 0.06rem;
        line-height: 0.18rem;
        color: $medium-fontColor;
        font-size: 0.12rem;
      }
    }
    &__icon {
      transform: rotate(180deg);
      position: absolute;
      right: 0.16rem;
      top: 0.55rem;
      color: $medium-fontColor;
      font-size: 0.2rem;
    }
  }
}
</style>
