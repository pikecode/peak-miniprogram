<template>
  <view class="page">
    <view class="header">
      <text class="title">购物袋</text>
    </view>
    <view class="content">
      <text class="tip">购物袋页面</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style lang="scss">
.page {
  min-height: 100vh;
  background: #f5f5f5;
  padding: 40rpx;

  .header {
    margin-bottom: 40rpx;

    .title {
      font-size: 48rpx;
      font-weight: 600;
      color: #000000;
    }
  }

  .content {
    text-align: center;
    padding: 200rpx 0;

    .tip {
      font-size: 28rpx;
      color: #999999;
    }
  }
}
</style>
