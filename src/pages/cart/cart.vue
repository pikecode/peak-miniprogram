<template>
  <view class="page">
    <!-- 购物车非空状态 -->
    <view v-if="cartItems.length > 0" class="cart-content">
      <!-- 购物车列表 -->
      <view class="cart-list">
        <view
          v-for="(item, index) in cartItems"
          :key="index"
          class="cart-item"
        >
          <!-- checkbox -->
          <view class="item-checkbox" @tap="toggleItemSelect(index)">
            <view class="checkbox" :class="{ checked: item.selected }">
              <text v-if="item.selected" class="checkbox-icon">✔</text>
            </view>
          </view>

          <!-- 产品信息 -->
          <view class="item-container">
            <image class="item-image" :src="item.image" mode="aspectFill"></image>

            <view class="item-details">
              <text class="item-name">{{ item.name }}</text>
              <text class="item-specs">颜色：{{ item.color }}</text>
              <text class="item-specs">尺码：{{ item.size }}</text>

              <view class="item-footer">
                <text class="item-price">¥{{ item.price }}</text>

                <view class="quantity-control">
                  <text class="qty-label">数量：</text>
                  <view class="qty-selector">
                    <view
                      class="qty-btn"
                      @tap="decreaseQuantity(index)"
                    >
                      <text>−</text>
                    </view>
                    <text class="qty-value">{{ item.quantity }}</text>
                    <view
                      class="qty-btn"
                      @tap="increaseQuantity(index)"
                    >
                      <text>+</text>
                    </view>
                  </view>
                </view>
              </view>
            </view>
          </view>

          <!-- 删除按钮 -->
          <view
            class="remove-btn"
            @tap="removeItem(index)"
          >
            <text>×</text>
          </view>
        </view>
      </view>

      <!-- 猜你喜欢推荐 -->
      <RecommendSection
        :items="recommendProducts"
        :columns="2"
        @product-tap="onProductTap"
        @favorite-change="onFavoriteChange"
      />
    </view>

    <!-- 底部固定栏 -->
    <view v-if="cartItems.length > 0" class="cart-footer">
      <view class="footer-left">
        <view class="select-all" @tap="toggleSelectAll">
          <view class="checkbox" :class="{ checked: isSelectAll }">
            <text v-if="isSelectAll" class="checkbox-icon">✔</text>
          </view>
          <text class="select-label">全选</text>
        </view>
        <view class="total-price-info">
          <text class="price-label">总计：</text>
          <text class="price-value">¥{{ selectedTotal }}</text>
        </view>
      </view>
      <view class="checkout-btn" @tap="handleCheckout">
        <text>立即结算({{ selectedCount }})</text>
      </view>
    </view>

    <!-- 空购物车状态 -->
    <view v-else class="cart-content">
      <view class="empty-cart-inner">
        <view class="empty-illustration">
          <text class="empty-icon">🛍️</text>
        </view>
        <text class="empty-title">购物袋为空</text>
        <text class="empty-description">快去选择您喜爱的商品吧</text>
        <view
          class="empty-action-btn"
          @tap="continueShopping"
        >
          <text>继续购物</text>
        </view>
      </view>

      <!-- 猜你喜欢推荐 -->
      <RecommendSection
        :items="recommendProducts"
        :columns="2"
        @product-tap="onProductTap"
        @favorite-change="onFavoriteChange"
      />
    </view>
  </view>
</template>

<script>
import RecommendSection from '../../components/RecommendSection.vue'

export default {
  components: {
    RecommendSection
  },
  data() {
    return {
      expressPrice: 0,
      discount: 0,
      cartItems: [
        {
          id: 1,
          name: '【明星同款】Prada Explore 中号Re-Nylon单肩包',
          color: '黑色',
          size: '均码',
          price: '17900',
          image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=400&q=80',
          quantity: 1,
          selected: true
        },
        {
          id: 4,
          name: '中号羊皮革双肩背包',
          color: '黑色',
          size: '均码',
          price: '35900',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80',
          quantity: 2,
          selected: true
        }
      ],
      recommendProducts: [
        {
          id: 1,
          name: '【明星同款】Prada Explore 中号Re-Nylon单肩包',
          price: '17,900',
          image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=400&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 2,
          name: '【预售】Prada Explore中号Nappa牛皮革单肩包',
          price: '26,400',
          image: 'https://images.unsplash.com/photo-1596736342875-ff5348bf9908?w=400&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 3,
          name: 'Re-Nylon双肩背包',
          price: '21,800',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80',
          imageCount: 2,
          isFavorite: false
        },
        {
          id: 4,
          name: '【预售】皮革中筒靴',
          price: '15,200',
          image: 'https://images.unsplash.com/photo-1543163521-1bf539c55dd2?w=400&q=80',
          imageCount: 2,
          isFavorite: false
        }
      ]
    }
  },
  computed: {
    selectedCount() {
      return this.cartItems.filter(item => item.selected).length
    },
    selectedTotal() {
      return this.cartItems
        .filter(item => item.selected)
        .reduce((sum, item) => sum + parseInt(item.price) * item.quantity, 0)
        .toString()
    },
    isSelectAll() {
      return this.cartItems.length > 0 && this.cartItems.every(item => item.selected)
    }
  },
  onLoad() {
    console.log('购物袋页面加载完成')
  },
  onShow() {
    // 合并待加入的商品（来自其他页面的“即刻购买”）
    try {
      const pending = uni.getStorageSync('pendingCartItems') || []
      if (Array.isArray(pending) && pending.length) {
        pending.forEach((p) => {
          const idx = this.cartItems.findIndex((x) => x.id === p.id)
          if (idx >= 0) {
            this.cartItems[idx].quantity += p.quantity || 1
          } else {
            this.cartItems.push(p)
          }
        })
        uni.removeStorageSync('pendingCartItems')
        this.$forceUpdate()
      }
    } catch (e) {}
  },
  methods: {
    toggleItemSelect(index) {
      this.cartItems[index].selected = !this.cartItems[index].selected
      this.$forceUpdate()
    },
    toggleSelectAll() {
      const allSelected = this.isSelectAll
      this.cartItems.forEach(item => {
        item.selected = !allSelected
      })
      this.$forceUpdate()
    },
    increaseQuantity(index) {
      this.cartItems[index].quantity++
      this.$forceUpdate()
    },
    decreaseQuantity(index) {
      if (this.cartItems[index].quantity > 1) {
        this.cartItems[index].quantity--
        this.$forceUpdate()
      }
    },
    removeItem(index) {
      uni.showModal({
        title: '确认删除',
        content: '是否确认删除此商品?',
        success: (res) => {
          if (res.confirm) {
            this.cartItems.splice(index, 1)
            this.$forceUpdate()
            uni.showToast({
              title: '已移出购物袋',
              icon: 'none',
              duration: 1500
            })
          }
        }
      })
    },
    continueShopping() {
      uni.switchTab({
        url: '/pages/index/index'
      })
    },
    handleCheckout() {
      uni.showToast({
        title: '前往支付',
        icon: 'none',
        duration: 1500
      })
      // 可以导航到订单确认页或支付页
      // uni.navigateTo({
      //   url: '/pages/checkout/checkout'
      // })
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
    onFavoriteChange({ index, isFavorite }) {
      const status = isFavorite ? '已收藏' : '已移除'
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
.page {
  height: 100vh;
  background: #ffffff;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* 购物车内容 */
.cart-content {
  display: flex;
  flex-direction: column;
  padding: 0;
  padding-bottom: 160rpx;
  position: relative;
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
}

/* 购物车列表 */
.cart-list {
  flex: 1;
  padding: 40rpx;

  .cart-item {
    display: flex;
    gap: 16rpx;
    padding: 24rpx;
    background: #ffffff;
    border: 1px solid #f0f0f0;
    border-radius: 8rpx;
    margin-bottom: 16rpx;
    position: relative;

    .item-checkbox {
      display: flex;
      align-items: flex-start;
      justify-content: center;
      padding-top: 12rpx;
      flex-shrink: 0;

      .checkbox {
        width: 24rpx;
        height: 24rpx;
        border: 2px solid #d0d0d0;
        border-radius: 4rpx;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.2s ease;
        cursor: pointer;

        .checkbox-icon {
          font-size: 14rpx;
          font-weight: 600;
          color: transparent;
          transition: all 0.2s ease;
        }

        &.checked {
          background: #000000;
          border-color: #000000;

          .checkbox-icon {
            color: #ffffff;
          }
        }
      }
    }

    .item-container {
      flex: 1;
      display: flex;
      gap: 16rpx;

      .item-image {
        width: 120rpx;
        height: 120rpx;
        background: #f5f5f5;
        border-radius: 4rpx;
        flex-shrink: 0;
      }

      .item-details {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        .item-name {
          display: block;
          font-size: 26rpx;
          font-weight: 500;
          color: #000000;
          margin-bottom: 8rpx;
          line-height: 1.4;
        }

        .item-specs {
          display: block;
          font-size: 22rpx;
          color: #999999;
          margin-bottom: 4rpx;
        }

        .item-footer {
          display: flex;
          align-items: flex-end;
          justify-content: space-between;
          margin-top: 8rpx;

          .item-price {
            font-size: 28rpx;
            font-weight: 600;
            color: #000000;
          }

          .quantity-control {
            display: flex;
            align-items: center;
            gap: 8rpx;

            .qty-label {
              font-size: 22rpx;
              color: #666666;
            }

            .qty-selector {
              display: flex;
              align-items: center;
              gap: 8rpx;
              border: 1px solid #d0d0d0;
              border-radius: 4rpx;
              padding: 4rpx;

              .qty-btn {
                width: 28rpx;
                height: 28rpx;
                display: flex;
                align-items: center;
                justify-content: center;
                font-size: 18rpx;
                color: #999999;
                cursor: pointer;

                &:active {
                  color: #333333;
                }
              }

              .qty-value {
                width: 32rpx;
                text-align: center;
                font-size: 20rpx;
                color: #333333;
              }
            }
          }
        }
      }
    }

    .remove-btn {
      position: absolute;
      top: 12rpx;
      right: 12rpx;
      width: 32rpx;
      height: 32rpx;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 28rpx;
      color: #999999;
      cursor: pointer;
      transition: all 0.2s ease;

      &:active {
        color: #333333;
      }
    }
  }
}

/* 购物车底部固定栏 */
.cart-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background: #ffffff;
  border-top: 1px solid #f0f0f0;
  padding: 16rpx 40rpx;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16rpx;
  z-index: 999;
  box-sizing: border-box;

  .footer-left {
    display: flex;
    align-items: center;
    gap: 24rpx;

    .select-all {
      display: flex;
      align-items: center;
      gap: 8rpx;
      cursor: pointer;

      .checkbox {
        width: 24rpx;
        height: 24rpx;
        border: 2px solid #d0d0d0;
        border-radius: 4rpx;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.2s ease;
        flex-shrink: 0;

        .checkbox-icon {
          font-size: 14rpx;
          font-weight: 600;
          color: transparent;
          transition: all 0.2s ease;
        }

        &.checked {
          background: #000000;
          border-color: #000000;

          .checkbox-icon {
            color: #ffffff;
          }
        }
      }

      .select-label {
        font-size: 28rpx;
        color: #333333;
      }
    }

    .total-price-info {
      display: flex;
      align-items: baseline;
      gap: 8rpx;

      .price-label {
        font-size: 24rpx;
        color: #666666;
      }

      .price-value {
        font-size: 32rpx;
        font-weight: 600;
        color: #000000;
      }
    }
  }

  .checkout-btn {
    flex-shrink: 0;
    background: #d0d0d0;
    color: #ffffff;
    padding: 16rpx 32rpx;
    border-radius: 8rpx;
    font-size: 28rpx;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
    min-width: 200rpx;
    text-align: center;

    &:active {
      background: #b0b0b0;
      transform: scale(0.98);
    }

    text {
      display: block;
    }
  }
}

/* 空购物车内容 */
.empty-cart-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 50vh;
  padding: 40rpx;

  .empty-illustration {
    margin-bottom: 40rpx;

    .empty-icon {
      font-size: 120rpx;
      display: block;
    }
  }

  .empty-title {
    display: block;
    font-size: 40rpx;
    font-weight: 600;
    color: #000000;
    margin-bottom: 16rpx;
    text-align: center;
  }

  .empty-description {
    display: block;
    font-size: 28rpx;
    color: #999999;
    margin-bottom: 60rpx;
    text-align: center;
  }

  .empty-action-btn {
    width: 100%;
    max-width: 400rpx;
    height: 88rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #000000;
    color: #ffffff;
    border-radius: 8rpx;
    font-size: 32rpx;
    font-weight: 600;
    cursor: pointer;

    &:active {
      background: #333333;
    }
  }
}
</style>
