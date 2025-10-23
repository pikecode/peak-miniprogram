<template>
  <view class="orders-page">
    <!-- 自定义导航栏 -->
    <view class="custom-navbar">
      <view class="navbar-left">
        <text class="back-btn" @tap="goBack">‹</text>
      </view>
      <text class="navbar-title">我的订单</text>
      <view class="navbar-right">
        <text class="navbar-icon">⋮</text>
        <text class="navbar-icon">−</text>
        <text class="navbar-icon">◯</text>
      </view>
    </view>

    <!-- 标签页 -->
    <view class="tabs-container">
      <view
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-item"
        :class="{ active: activeTab === index }"
        @tap="selectTab(index)"
      >
        <text>{{ tab.label }}</text>
      </view>
      <view class="tab-indicator" :style="{ left: activeTab * 20 + '%' }"></view>
    </view>

    <!-- 内容区域 -->
    <view class="orders-content">
      <!-- 空状态 -->
      <view v-if="!hasOrders" class="empty-state">
        <text class="empty-icon">📋</text>
        <text class="empty-text">您还没有相关订单</text>
        <view class="shop-btn" @tap="goShopping">
          <text>立即选购</text>
        </view>
      </view>

      <!-- 订单列表（当有订单时显示） -->
      <view v-else class="orders-list">
        <view
          v-for="(order, index) in filteredOrders"
          :key="index"
          class="order-card"
        >
          <view class="order-header">
            <text class="order-status" :class="order.status">{{ order.statusText }}</text>
            <text class="order-date">{{ order.date }}</text>
          </view>
          <view class="order-items">
            <view
              v-for="(item, itemIndex) in order.items"
              :key="itemIndex"
              class="order-item"
            >
              <image :src="item.image" class="item-image"></image>
              <view class="item-info">
                <text class="item-name">{{ item.name }}</text>
                <text class="item-price">¥{{ item.price }}</text>
              </view>
              <text class="item-quantity">x{{ item.quantity }}</text>
            </view>
          </view>
          <view class="order-footer">
            <text class="total-price">共{{ order.totalItems }}件 合计：¥{{ order.totalPrice }}</text>
            <view class="order-actions">
              <view class="action-btn" @tap="onOrderAction('detail', order)">
                <text>订单详情</text>
              </view>
            </view>
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
      activeTab: 0,
      tabs: [
        { label: '全部', status: 'all' },
        { label: '待支付', status: 'pending-payment' },
        { label: '待发货', status: 'pending-shipment' },
        { label: '已发货', status: 'shipped' },
        { label: '售后', status: 'aftersales' }
      ],
      orders: [
        {
          id: '2024101001',
          status: 'pending-payment',
          statusText: '待支付',
          date: '2024-10-10',
          totalItems: 1,
          totalPrice: '17,900',
          items: [
            {
              name: '【粉星同款】Prada Explore 中号Re-Nylon单肩包',
              price: '17,900',
              quantity: 1,
              image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=100&q=80'
            }
          ]
        },
        {
          id: '2024100901',
          status: 'pending-shipment',
          statusText: '待发货',
          date: '2024-10-09',
          totalItems: 2,
          totalPrice: '44,300',
          items: [
            {
              name: '【特售】Prada Explore中号Nappa牛皮革单肩包',
              price: '26,400',
              quantity: 1,
              image: 'https://images.unsplash.com/photo-1596736342875-ff5348bf9908?w=100&q=80'
            },
            {
              name: 'Re-Nylon双肩背包',
              price: '19,500',
              quantity: 1,
              image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=100&q=80'
            }
          ]
        }
      ],
      hasOrders: false
    }
  },
  computed: {
    filteredOrders() {
      if (this.activeTab === 0) {
        return this.orders
      }
      const statusFilter = this.tabs[this.activeTab].status
      return this.orders.filter(order => order.status === statusFilter)
    }
  },
  onLoad(options) {
    console.log('订单页面加载完成')
    // 根据路由参数设置默认tab
    if (options.status && options.status !== 'all') {
      const statusIndex = this.tabs.findIndex(tab => tab.status === options.status)
      if (statusIndex !== -1) {
        this.activeTab = statusIndex
      }
    }
    // 模拟订单数据，如果当前选项卡没有订单，则显示空状态
    this.updateOrdersDisplay()
  },
  methods: {
    selectTab(index) {
      this.activeTab = index
      this.updateOrdersDisplay()
    },
    updateOrdersDisplay() {
      this.hasOrders = this.filteredOrders.length > 0
    },
    goBack() {
      uni.navigateBack()
    },
    goShopping() {
      uni.switchTab({
        url: '/pages/index/index'
      })
    },
    onOrderAction(action, order) {
      if (action === 'detail') {
        uni.showToast({
          title: `订单 ${order.id}`,
          icon: 'none',
          duration: 1500
        })
        // 可以导航到订单详情页
        // uni.navigateTo({
        //   url: `/pages/orders/detail?id=${order.id}`
        // })
      }
    }
  }
}
</script>

<style lang="scss">
.orders-page {
  min-height: 100vh;
  background: #ffffff;
}

/* 自定义导航栏 */
.custom-navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16rpx 0;
  padding-top: 32rpx;
  background: #ffffff;
  border-bottom: 1px solid #f0f0f0;
  position: sticky;
  top: 0;
  z-index: 100;

  .navbar-left {
    flex: 1;
    padding-left: 24rpx;
  }

  .back-btn {
    display: block;
    font-size: 48rpx;
    color: #000000;
    cursor: pointer;
    line-height: 1;
  }

  .navbar-title {
    display: block;
    font-size: 32rpx;
    font-weight: 600;
    color: #000000;
    flex: 2;
    text-align: center;
  }

  .navbar-right {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 16rpx;
    padding-right: 24rpx;

    .navbar-icon {
      display: block;
      font-size: 32rpx;
      color: #000000;
      cursor: pointer;
      line-height: 1;
    }
  }
}

/* 标签页 */
.tabs-container {
  position: relative;
  display: flex;
  background: #ffffff;
  border-bottom: 1px solid #f0f0f0;
  overflow-x: auto;
  overflow-y: hidden;

  .tab-item {
    flex: 1;
    text-align: center;
    padding: 20rpx 0;
    font-size: 26rpx;
    color: #999999;
    font-weight: 400;
    white-space: nowrap;
    cursor: pointer;
    transition: color 0.3s ease;

    &.active {
      color: #000000;
      font-weight: 500;
    }
  }

  .tab-indicator {
    position: absolute;
    bottom: 0;
    height: 4rpx;
    width: 20%;
    background: #000000;
    transition: left 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    border-radius: 2rpx;
  }
}

/* 订单内容区域 */
.orders-content {
  min-height: calc(100vh - 200rpx);
  padding: 40rpx;
  background: #ffffff;
}

/* 空状态 */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 40rpx;
  gap: 24rpx;

  .empty-icon {
    display: block;
    font-size: 80rpx;
    margin-bottom: 20rpx;
  }

  .empty-text {
    display: block;
    font-size: 28rpx;
    color: #999999;
    font-weight: 400;
  }

  .shop-btn {
    margin-top: 40rpx;
    padding: 20rpx 60rpx;
    background: #000000;
    color: #ffffff;
    border-radius: 8rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s ease;

    &:active {
      background: #333333;
      transform: scale(0.98);
    }

    text {
      display: block;
      font-size: 28rpx;
      font-weight: 500;
    }
  }
}

/* 订单列表 */
.orders-list {
  display: flex;
  flex-direction: column;
  gap: 24rpx;

  .order-card {
    background: #ffffff;
    border: 1px solid #f0f0f0;
    border-radius: 8rpx;
    overflow: hidden;
    transition: all 0.3s ease;

    &:active {
      box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
    }

    .order-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16rpx 20rpx;
      background: #f9f9f9;
      border-bottom: 1px solid #f0f0f0;

      .order-status {
        display: block;
        font-size: 24rpx;
        font-weight: 500;
        color: #666666;

        &.pending-payment {
          color: #ff6b35;
        }

        &.pending-shipment {
          color: #ffa500;
        }

        &.shipped {
          color: #4285f4;
        }

        &.aftersales {
          color: #999999;
        }
      }

      .order-date {
        display: block;
        font-size: 22rpx;
        color: #999999;
      }
    }

    .order-items {
      padding: 20rpx;
      border-bottom: 1px solid #f0f0f0;

      .order-item {
        display: flex;
        gap: 12rpx;
        margin-bottom: 16rpx;

        &:last-child {
          margin-bottom: 0;
        }

        .item-image {
          width: 80rpx;
          height: 80rpx;
          background: #f5f5f5;
          border-radius: 4rpx;
          display: block;
        }

        .item-info {
          flex: 1;
          display: flex;
          flex-direction: column;
          justify-content: center;

          .item-name {
            display: block;
            font-size: 24rpx;
            color: #333333;
            margin-bottom: 8rpx;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
          }

          .item-price {
            display: block;
            font-size: 24rpx;
            color: #000000;
            font-weight: 600;
          }
        }

        .item-quantity {
          display: block;
          font-size: 24rpx;
          color: #999999;
          align-self: center;
          white-space: nowrap;
        }
      }
    }

    .order-footer {
      padding: 16rpx 20rpx;
      display: flex;
      justify-content: space-between;
      align-items: center;

      .total-price {
        display: block;
        font-size: 24rpx;
        color: #333333;
      }

      .order-actions {
        display: flex;
        gap: 12rpx;

        .action-btn {
          padding: 12rpx 20rpx;
          border: 1px solid #000000;
          border-radius: 4rpx;
          cursor: pointer;
          transition: all 0.3s ease;

          &:active {
            background: #000000;

            text {
              color: #ffffff;
            }
          }

          text {
            display: block;
            font-size: 22rpx;
            color: #000000;
            font-weight: 400;
          }
        }
      }
    }
  }
}
</style>
