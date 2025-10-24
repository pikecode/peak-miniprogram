<template>
  <view class="addresses-page">
    <!-- 页面内容 -->
    <view class="addresses-content">
      <!-- 地址卡片 -->
      <view class="address-card">
        <!-- 编辑和删除按钮 -->
        <view class="card-buttons">
          <text class="edit-btn" @tap="onEditAddress">编辑</text>
          <text class="delete-btn" @tap="onDeleteAddress">×</text>
        </view>

        <!-- 用户信息 -->
        <view class="user-info">
          <view class="name-section">
            <text class="user-name">王先生</text>
            <view class="default-badge">默认</view>
          </view>
          <text class="user-phone">186****825</text>
        </view>

        <!-- 地址详情 -->
        <view class="address-details">
          <view class="detail-row">
            <text class="detail-label">省份:</text>
            <text class="detail-value">北京市</text>
          </view>
          <view class="detail-row">
            <text class="detail-label">城市:</text>
            <text class="detail-value">东城区</text>
          </view>
          <view class="detail-row">
            <text class="detail-label">地址:</text>
            <text class="detail-value">王府井大街100号</text>
          </view>
        </view>

        <!-- 完整地址 -->
        <view class="full-address">
          <text>北京市 东城区 王府井大街100号</text>
        </view>
      </view>

      <!-- 空状态提示 -->
      <view v-if="addressList.length === 0" class="empty-state">
        <text class="empty-text">暂无地址，请添加收货地址</text>
      </view>
    </view>

    <!-- 底部按钮 -->
    <view class="bottom-button">
      <view class="add-address-btn" @tap="onAddAddress">
        <text>添加地址</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      addressList: [
        {
          id: 1,
          name: '王先生',
          phone: '186****825',
          province: '北京市',
          city: '东城区',
          address: '王府井大街100号',
          isDefault: true
        }
      ]
    }
  },
  onLoad() {
    console.log('地址簿页面加载完成')
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    onEditAddress() {
      uni.navigateTo({
        url: '/pages/addresses/add-edit?id=1'
      })
    },
    onDeleteAddress() {
      uni.showModal({
        title: '删除地址',
        content: '确定要删除这个地址吗？',
        success: (res) => {
          if (res.confirm) {
            uni.showToast({
              title: '地址已删除',
              icon: 'none',
              duration: 1000
            })
            // 实际删除逻辑
            this.addressList.shift()
          }
        }
      })
    },
    onAddAddress() {
      uni.navigateTo({
        url: '/pages/addresses/add-edit'
      })
    }
  }
}
</script>

<style lang="scss">
.addresses-page {
  min-height: 100vh;
  background: #ffffff;
  display: flex;
  flex-direction: column;
  padding-bottom: 120rpx;
}

/* 页面内容 */
.addresses-content {
  flex: 1;
  padding: 40rpx;
  padding-top: 20rpx;
}

/* 地址卡片 */
.address-card {
  position: relative;
  background: #ffffff;
  border: 1px solid #f0f0f0;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 24rpx;
}

/* 卡片顶部按钮 */
.card-buttons {
  position: absolute;
  top: 20rpx;
  right: 20rpx;
  display: flex;
  gap: 16rpx;
  align-items: center;

  .edit-btn {
    font-size: 24rpx;
    color: #666666;
    padding: 6rpx 12rpx;
    border: 1px solid #d0d0d0;
    border-radius: 4rpx;
    transition: all 0.2s ease;

    &:active {
      background: #f5f5f5;
      transform: scale(0.95);
    }
  }

  .delete-btn {
    font-size: 32rpx;
    color: #999999;
    font-weight: 300;
    transition: all 0.2s ease;

    &:active {
      color: #666666;
      transform: scale(1.2);
    }
  }
}

/* 用户信息 */
.user-info {
  margin-bottom: 28rpx;
  padding-right: 100rpx;

  .name-section {
    display: flex;
    align-items: center;
    gap: 12rpx;
    margin-bottom: 12rpx;

    .user-name {
      font-size: 28rpx;
      font-weight: 600;
      color: #000000;
    }

    .default-badge {
      background: #000000;
      color: #ffffff;
      font-size: 20rpx;
      padding: 4rpx 10rpx;
      border-radius: 4rpx;
      font-weight: 500;
    }
  }

  .user-phone {
    display: block;
    font-size: 24rpx;
    color: #999999;
  }
}

/* 地址详情 */
.address-details {
  margin-bottom: 24rpx;
  padding-bottom: 24rpx;
  border-bottom: 1px solid #f0f0f0;

  .detail-row {
    display: flex;
    margin-bottom: 16rpx;
    font-size: 24rpx;

    &:last-child {
      margin-bottom: 0;
    }

    .detail-label {
      width: 80rpx;
      color: #999999;
      min-width: 80rpx;
    }

    .detail-value {
      color: #333333;
      flex: 1;
    }
  }
}

/* 完整地址 */
.full-address {
  text {
    display: block;
    font-size: 24rpx;
    color: #666666;
    line-height: 1.6;
  }
}

/* 空状态 */
.empty-state {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 300rpx;

  .empty-text {
    font-size: 28rpx;
    color: #999999;
  }
}

/* 底部按钮 */
.bottom-button {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #ffffff;
  border-top: 1px solid #f0f0f0;
  padding: 24rpx 40rpx;
  padding-bottom: max(24rpx, env(safe-area-inset-bottom));

  .add-address-btn {
    width: 100%;
    padding: 20rpx 0;
    background: #000000;
    color: #ffffff;
    border-radius: 8rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 28rpx;
    font-weight: 500;
    transition: all 0.2s ease;

    &:active {
      background: #333333;
      transform: scale(0.98);
    }

    text {
      display: block;
    }
  }
}
</style>
