<template>
  <view class="profile-page">
    <!-- 轮播图区域（包含动画和其他banner） -->
    <view class="banner-section">
      <swiper
        class="banner-swiper"
        :indicator-dots="true"
        :indicator-color="indicatorColor"
        :indicator-active-color="indicatorActiveColor"
        :autoplay="true"
        :interval="5000"
        :circular="true"
        @change="onSwiperChange"
      >
        <!-- 轮播项 -->
        <swiper-item v-for="(banner, index) in banners" :key="index">
          <view class="banner-item">
            <image :src="banner.image" class="banner-image" mode="aspectFill"></image>
            <view class="banner-text-overlay">
              <text class="banner-brand">RUIZHU</text>
              <view class="banner-welcome">
                <text class="welcome-title">欢迎</text>
                <text class="welcome-desc">{{ userGreeting }}先生，您好</text>
              </view>
            </view>
          </view>
        </swiper-item>
      </swiper>
    </view>

    <!-- 我的订单 -->
    <view class="orders-card">
      <view class="orders-header" @tap="goToOrders('all')">
        <text class="orders-title">我的订单</text>
        <text class="orders-arrow">→</text>
      </view>
      <view class="order-status-grid">
        <view
          v-for="(status, index) in orderStatuses"
          :key="index"
          class="order-status-item"
          @tap="onOrderStatusTap(status)"
        >
          <view class="status-icon-wrapper">
            <text class="status-icon">{{ status.icon }}</text>
          </view>
          <text class="status-label">{{ status.label }}</text>
        </view>
      </view>
    </view>

    <!-- 快速访问 -->
    <view class="quick-access-section">
      <view class="quick-access-item" @tap="onQuickAccessTap('wishlist')">
        <text class="quick-access-icon">❤️</text>
        <text class="quick-access-label">我的心愿单</text>
      </view>
      <view class="quick-access-item" @tap="onQuickAccessTap('addresses')">
        <text class="quick-access-icon">📍</text>
        <text class="quick-access-label">我的地址簿</text>
      </view>
    </view>

    <!-- 法律和授权 -->
    <view class="legal-access-section">
      <view class="legal-item" @tap="onLegalTap('terms')">
        <text class="legal-icon">🛡️</text>
        <text class="legal-label">法律条款</text>
      </view>
      <view class="legal-item" @tap="onLegalTap('privacy')">
        <text class="legal-icon">👤</text>
        <text class="legal-label">个人信息授权</text>
      </view>
    </view>

    <!-- 猜你喜欢推荐 -->
    <view class="recommend-section">
      <text class="recommend-title">猜你喜欢</text>
      <view class="recommend-grid">
        <view
          v-for="(item, index) in recommendProducts"
          :key="index"
          class="recommend-card"
          @tap="onProductTap(item)"
        >
          <view class="recommend-image-wrapper">
            <image :src="item.image" class="recommend-image" mode="aspectFill"></image>
            <text class="favorite-btn" @tap.stop="toggleFavorite(index)">{{ item.isFavorite ? '❤️' : '🤍' }}</text>
            <view class="image-indicators">
              <text
                v-for="(dot, dotIndex) in item.imageCount"
                :key="dotIndex"
                class="indicator-dot"
                :class="{ active: dotIndex === 0 }"
              ></text>
            </view>
          </view>
          <view class="recommend-info">
            <text class="recommend-product-name">{{ item.name }}</text>
            <text class="recommend-price">¥{{ item.price }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      appVersion: '1.0.0',
      userGreeting: '张**',
      indicatorColor: 'rgba(255, 255, 255, 0.5)',
      indicatorActiveColor: '#ffffff',
      currentBannerIndex: 0,
      banners: [
        {
          image: 'https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800&q=80'
        },
        {
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=800&q=80'
        },
        {
          image: 'https://images.unsplash.com/photo-1572635196237-14b3f281503f?w=800&q=80'
        }
      ],
      orderStatuses: [
        { id: 'pending-payment', label: '待支付', icon: '💼' },
        { id: 'pending-shipment', label: '待发货', icon: '📦' },
        { id: 'shipped', label: '已发货', icon: '🚚' },
        { id: 'aftersales', label: '售后', icon: '📋' }
      ],
      recommendProducts: [
        {
          id: 1,
          name: '【粉星同款】Prada Explore 中号Re-Nylon单肩包',
          price: '17,900',
          image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=300&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 2,
          name: '【特售】Prada Explore中号Nappa牛皮革单肩包',
          price: '26,400',
          image: 'https://images.unsplash.com/photo-1596736342875-ff5348bf9908?w=300&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 3,
          name: 'Re-Nylon双肩背包',
          price: '19,500',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=300&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 4,
          name: '【特售】皮靴中筒靴',
          price: '8,900',
          image: 'https://images.unsplash.com/photo-1543163521-1bf539c55dd2?w=300&q=80',
          imageCount: 2,
          isFavorite: false
        }
      ]
    }
  },
  onLoad() {
    console.log('我的页面加载完成')
  },
  methods: {
    onSwiperChange(e) {
      this.currentBannerIndex = e.detail.current
    },
    onOrderStatusTap(status) {
      uni.navigateTo({
        url: `/pages/orders/orders?status=${status.id}`
      })
    },
    goToOrders(type) {
      uni.navigateTo({
        url: `/pages/orders/orders?status=${type}`
      })
    },
    onQuickAccessTap(type) {
      if (type === 'wishlist') {
        uni.navigateTo({
          url: '/pages/wishlist/wishlist'
        })
      } else if (type === 'addresses') {
        uni.navigateTo({
          url: '/pages/addresses/addresses'
        })
      }
    },
    onLegalTap(type) {
      if (type === 'terms') {
        uni.navigateTo({
          url: '/pages/legal/legal'
        })
      } else if (type === 'privacy') {
        uni.navigateTo({
          url: '/pages/legal/authorization'
        })
      }
    },
    onProductTap(item) {
      uni.showToast({
        title: item.name,
        icon: 'none',
        duration: 1500
      })
      // 可以导航到产品详情页
      // uni.navigateTo({
      //   url: `/pages/product/detail?id=${item.id}`
      // })
    },
    toggleFavorite(index) {
      this.recommendProducts[index].isFavorite = !this.recommendProducts[index].isFavorite
      const status = this.recommendProducts[index].isFavorite ? '已收藏' : '已移除'
      uni.showToast({
        title: status,
        icon: 'none',
        duration: 1000
      })
    }
  }
}
</script>

<style lang="scss">
.profile-page {
  min-height: 100vh;
  background: #ffffff;
  padding-bottom: 120rpx;
}

/* 轮播图区域 */
.banner-section {
  width: 100%;
  height: 920rpx;

  .banner-swiper {
    width: 100%;
    height: 100%;
  }

  .banner-item {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .banner-image {
    width: 100%;
    height: 100%;
  }

  .banner-text-overlay {
    position: absolute;
    bottom: 100rpx;
    left: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 2;

    .banner-brand {
      display: block;
      font-size: 56rpx;
      font-weight: 500;
      color: #ffffff;
      letter-spacing: 2rpx;
      margin-bottom: 20rpx;
      text-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.3);
    }

    .banner-welcome {
      display: flex;
      flex-direction: column;
      align-items: center;

      .welcome-title {
        display: block;
        font-size: 28rpx;
        color: #ffffff;
        letter-spacing: 1rpx;
        margin-bottom: 12rpx;
      }

      .welcome-desc {
        display: block;
        font-size: 28rpx;
        color: #ffffff;
        letter-spacing: 1rpx;
      }
    }
  }
}

/* 我的订单卡片 */
.orders-card {
  margin: 0 40rpx;
  margin-top: -80rpx;
  background: #ffffff; 
  padding: 32rpx 24rpx;
  position: relative;
  z-index: 10;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);

  .orders-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 32rpx;
    padding-bottom: 24rpx;
    border-bottom: 1px solid #f0f0f0;

    .orders-title {
      display: block;
      font-size: 32rpx;
      font-weight: 600;
      color: #000000;
    }

    .orders-arrow {
      display: block;
      font-size: 32rpx;
      color: #000000;
      font-weight: 300;
    }
  }

  .order-status-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20rpx;

    .order-status-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12rpx;
      cursor: pointer;
      transition: all 0.3s ease;

      &:active {
        transform: scale(0.95);
      }

      .status-icon-wrapper {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 64rpx;
        height: 64rpx;
        
      

        .status-icon {
          display: block;
          font-size: 36rpx;
        }
      }

      .status-label {
        display: block;
        font-size: 22rpx;
        color: #333333;
        text-align: center;
        font-weight: 400;
      }

      &:active .status-icon-wrapper {
        border-color: #000000;
        background: #f9f9f9;
      }
    }
  }
}

/* 快速访问区域 */
.quick-access-section {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16rpx;
  padding: 40rpx 40rpx 0;
  margin-top: 32rpx;

  .quick-access-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40rpx 24rpx;
    background: #ffffff;
    border: 1px solid #f0f0f0;
    border-radius: 8rpx;
    cursor: pointer;
    transition: all 0.3s ease;

    &:active {
      background: #f9f9f9;
      border-color: #000000;
      transform: scale(0.98);
    }

    .quick-access-icon {
      display: block;
      font-size: 48rpx;
      margin-bottom: 16rpx;
    }

    .quick-access-label {
      display: block;
      font-size: 26rpx;
      color: #333333;
      font-weight: 400;
      text-align: center;
    }
  }
}

/* 法律授权区域 */
.legal-access-section {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16rpx;
  padding: 0 40rpx;
  margin-top: 20rpx;
  margin-bottom: 40rpx;

  .legal-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40rpx 24rpx;
    background: #ffffff;
    border: 1px solid #f0f0f0;
    border-radius: 8rpx;
    cursor: pointer;
    transition: all 0.3s ease;

    &:active {
      background: #f9f9f9;
      border-color: #000000;
      transform: scale(0.98);
    }

    .legal-icon {
      display: block;
      font-size: 48rpx;
      margin-bottom: 16rpx;
    }

    .legal-label {
      display: block;
      font-size: 26rpx;
      color: #333333;
      font-weight: 400;
      text-align: center;
    }
  }
}

/* 猜你喜欢推荐 */
.recommend-section {
  padding: 40rpx;
  background: #ffffff;

  .recommend-title {
    display: block;
    font-size: 32rpx;
    font-weight: 600;
    color: #000000;
    margin-bottom: 32rpx;
  }

  .recommend-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24rpx;

    .recommend-card {
      display: flex;
      flex-direction: column;
      cursor: pointer;
      transition: all 0.3s ease;

      &:active {
        transform: scale(0.98);
      }

      .recommend-image-wrapper {
        position: relative;
        width: 100%;
        height: 320rpx;
        background: #f5f5f5;
        border-radius: 8rpx;
        overflow: hidden;
        margin-bottom: 16rpx;

        .recommend-image {
          width: 100%;
          height: 100%;
          display: block;
        }

        .favorite-btn {
          position: absolute;
          top: 12rpx;
          right: 12rpx;
          font-size: 32rpx;
          cursor: pointer;
          transition: all 0.2s ease;
          z-index: 5;

          &:active {
            transform: scale(1.1);
          }
        }

        .image-indicators {
          position: absolute;
          bottom: 12rpx;
          left: 50%;
          transform: translateX(-50%);
          display: flex;
          gap: 6rpx;

          .indicator-dot {
            display: block;
            width: 8rpx;
            height: 8rpx;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;

            &.active {
              background: #ffffff;
              width: 24rpx;
              border-radius: 4rpx;
            }
          }
        }
      }

      .recommend-info {
        display: flex;
        flex-direction: column;
        gap: 8rpx;

        .recommend-product-name {
          display: block;
          font-size: 24rpx;
          color: #333333;
          font-weight: 400;
          line-height: 1.4;
          overflow: hidden;
          text-overflow: ellipsis;
          display: -webkit-box;
          -webkit-line-clamp: 2;
          -webkit-box-orient: vertical;
        }

        .recommend-price {
          display: block;
          font-size: 28rpx;
          color: #000000;
          font-weight: 600;
        }
      }
    }
  }
}
</style>
