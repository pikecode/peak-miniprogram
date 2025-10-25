<template>
  <view class="page">
    <!-- 自定义顶部导航栏 -->
    <view class="custom-navbar">
      <view class="navbar-content">
        <!-- 左侧：返回与首页 -->
        <view class="nav-pill nav-pill-left">
          <view class="nav-icon-button" @tap="goBack">
            <image class="icon-arrow-left" src="/static/icons/arrow-left.svg" mode="widthFix"></image>
          </view>
          <view class="nav-divider"></view>
          <view class="nav-icon-button" @tap="goHome">
            <image class="icon-home" src="/static/icons/home-outline.svg" mode="widthFix"></image>
          </view>
        </view>

        <!-- 中间：页面标题 -->
        <text class="nav-title">Prada Natural系列</text>
      </view>
    </view>

    <!-- 页面标题 -->
    <view class="header-section">
      <text class="title">专属定制服务</text>
      <text class="subtitle">选择您感兴趣的产品开启定制之旅</text>
    </view>

    <!-- 产品分类 -->
    <view class="category-tabs">
      <view
        v-for="(category, index) in categories"
        :key="index"
        class="category-tab"
        :class="{ active: activeCategory === index }"
        @tap="activeCategory = index"
      >
        <text>{{ category.name }}</text>
      </view>
    </view>

    <!-- 产品网格 -->
    <view class="products-container">
      <view v-if="filteredProducts.length > 0" class="product-grid">
        <view
          v-for="(product, index) in filteredProducts"
          :key="index"
          class="product-card"
          @tap="onProductSelect(product)"
        >
          <!-- 产品图片 -->
          <view class="product-image-wrapper">
            <image class="product-image" :src="product.image" mode="aspectFill"></image>
            <view class="badge">{{ product.isNew ? '新品' : '' }}</view>
            <text class="favorite-icon" @tap.stop="toggleFavorite(index)">
              {{ product.isFavorite ? '♥' : '♡' }}
            </text>
          </view>

          <!-- 产品信息 -->
          <view class="product-info">
            <text class="product-name">{{ product.name }}</text>
            <text class="product-color">{{ product.color }}</text>
            <text class="product-price">¥{{ product.price }}</text>
          </view>

          <!-- 颜色选择 -->
          <view class="color-dots">
            <view
              v-for="(color, i) in product.colors"
              :key="i"
              class="color-dot"
              :style="{ backgroundColor: color.value }"
              :title="color.name"
            ></view>
          </view>
        </view>
      </view>

      <!-- 空状态 -->
      <view v-else class="empty-state">
        <text class="empty-text">暂无产品</text>
      </view>
    </view>

    <!-- 咨询表单（浮窗） -->
    <view class="consultation-form" v-if="selectedProduct">
      <view class="form-header">
        <text class="form-title">定制咨询</text>
        <text class="close-btn" @tap="selectedProduct = null">✕</text>
      </view>

      <view class="form-content">
        <!-- 产品预览 -->
        <view class="product-preview">
          <image :src="selectedProduct.image" mode="aspectFit"></image>
          <text class="preview-name">{{ selectedProduct.name }}</text>
        </view>

        <!-- 咨询表单 -->
        <view class="form-group">
          <text class="label">姓名 *</text>
          <input v-model="consultForm.name" type="text" placeholder="请输入您的姓名" />
        </view>

        <view class="form-group">
          <text class="label">电话 *</text>
          <input v-model="consultForm.phone" type="tel" placeholder="请输入您的电话" />
        </view>

        <view class="form-group">
          <text class="label">邮箱</text>
          <input v-model="consultForm.email" type="email" placeholder="请输入您的邮箱" />
        </view>

        <view class="form-group">
          <text class="label">选择颜色</text>
          <picker
            :range="selectedProduct.colors"
            :value="consultForm.colorIndex"
            @change="onColorChange"
            range-key="name"
          >
            <view class="picker-wrapper">
              <text class="picker-value">
                {{ selectedProduct.colors[consultForm.colorIndex]?.name || '请选择颜色' }}
              </text>
              <text class="picker-arrow">›</text>
            </view>
          </picker>
        </view>

        <view class="form-group">
          <text class="label">备注信息</text>
          <textarea
            v-model="consultForm.remarks"
            placeholder="请输入您的定制需求或特殊要求"
            maxlength="200"
          ></textarea>
          <text class="char-count">{{ consultForm.remarks.length }}/200</text>
        </view>

        <!-- 提交按钮 -->
        <view class="form-actions">
          <view class="cancel-btn" @tap="selectedProduct = null">
            <text>取消</text>
          </view>
          <view class="submit-btn" @tap="submitConsultation">
            <text>提交咨询</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 遮罩层 -->
    <view v-if="selectedProduct" class="mask" @tap="selectedProduct = null"></view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activeCategory: 0,
      selectedProduct: null,
      consultForm: {
        name: '',
        phone: '',
        email: '',
        colorIndex: 0,
        remarks: ''
      },
      categories: [
        { id: 'all', name: '全部产品' },
        { id: 'bag', name: '背包' },
        { id: 'clothing', name: '服装' },
        { id: 'shoes', name: '鞋履' }
      ],
      products: [
        // 背包系列
        {
          id: 1,
          name: 'Re-Nylon双肩背包',
          color: '黑色',
          price: '21,800',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80',
          category: 'bag',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '黑色', value: '#000000' },
            { name: '棕色', value: '#8B4513' }
          ]
        },
        {
          id: 2,
          name: 'Re-Nylon双肩背包',
          color: '棕色',
          price: '21,800',
          image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=400&q=80',
          category: 'bag',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '黑色', value: '#000000' },
            { name: '棕色', value: '#8B4513' }
          ]
        },
        {
          id: 3,
          name: 'Re-Nylon与牛皮革拼接双肩背包',
          color: '黑色',
          price: '28,700',
          image: 'https://images.unsplash.com/photo-1556821552-5f06b5991ce0?w=400&q=80',
          category: 'bag',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '黑色', value: '#000000' },
            { name: '深棕', value: '#654321' }
          ]
        },
        // 服装系列
        {
          id: 4,
          name: '再生尼龙羽绒夹克',
          color: '棕色',
          price: '28,400',
          image: 'https://images.unsplash.com/photo-1551028719-00167b16ebc5?w=400&q=80',
          category: 'clothing',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '棕色', value: '#8B4513' },
            { name: '黑色', value: '#000000' },
            { name: '深绿', value: '#2F5233' }
          ]
        },
        {
          id: 5,
          name: '黑色羊皮夹克',
          color: '黑色',
          price: '32,500',
          image: 'https://images.unsplash.com/photo-1591047990366-ebc4de0ece35?w=400&q=80',
          category: 'clothing',
          isNew: false,
          isFavorite: false,
          colors: [
            { name: '黑色', value: '#000000' },
            { name: '深棕', value: '#654321' }
          ]
        },
        // 鞋履系列
        {
          id: 6,
          name: '亮面牛皮革乐福鞋',
          color: '棕色',
          price: '10,300',
          image: 'https://images.unsplash.com/photo-1509631179647-0177331693ae?w=400&q=80',
          category: 'shoes',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '棕色', value: '#8B4513' },
            { name: '黑色', value: '#000000' }
          ]
        },
        {
          id: 7,
          name: '皮革中筒靴',
          color: '黑色',
          price: '15,800',
          image: 'https://images.unsplash.com/photo-1543163521-1bf539c55dd2?w=400&q=80',
          category: 'shoes',
          isNew: false,
          isFavorite: false,
          colors: [
            { name: '黑色', value: '#000000' },
            { name: '棕色', value: '#8B4513' }
          ]
        },
        {
          id: 8,
          name: '高跟皮革靴',
          color: '红棕',
          price: '12,900',
          image: 'https://images.unsplash.com/photo-1608256543803-ba4f8c70ae0b?w=400&q=80',
          category: 'shoes',
          isNew: true,
          isFavorite: false,
          colors: [
            { name: '红棕', value: '#8B4513' },
            { name: '黑色', value: '#000000' }
          ]
        }
      ]
    }
  },
  computed: {
    filteredProducts() {
      // 使用 activeCategory 作为缓存键
      if (this.activeCategory === 0) {
        return this.products
      }
      const selectedCategory = this.categories[this.activeCategory].id
      // 使用 filter 时避免创建中间数组，直接返回结果
      return this.products.filter(p => p.category === selectedCategory)
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    goHome() {
      uni.switchTab({
        url: '/pages/index/index'
      })
    },
    onProductSelect(product) {
      this.selectedProduct = product
      this.consultForm = {
        name: '',
        phone: '',
        email: '',
        colorIndex: 0,
        remarks: ''
      }
    },
    toggleFavorite(index) {
      // 在原始 products 数组中查找和更新对应的产品
      const product = this.filteredProducts[index]
      if (product) {
        const originalIndex = this.products.findIndex(p => p.id === product.id)
        if (originalIndex !== -1) {
          this.$set(this.products[originalIndex], 'isFavorite', !this.products[originalIndex].isFavorite)
          uni.showToast({
            title: this.products[originalIndex].isFavorite ? '已收藏' : '已移除收藏',
            icon: 'none',
            duration: 1000
          })
        }
      }
    },
    onColorChange(e) {
      this.consultForm.colorIndex = e.detail.value
    },
    submitConsultation() {
      // 验证表单
      if (!this.consultForm.name.trim()) {
        uni.showToast({
          title: '请输入姓名',
          icon: 'none'
        })
        return
      }

      if (!this.consultForm.phone.trim()) {
        uni.showToast({
          title: '请输入电话',
          icon: 'none'
        })
        return
      }

      // 验证电话格式
      const phoneRegex = /^1[3-9]\d{9}$/
      if (!phoneRegex.test(this.consultForm.phone)) {
        uni.showToast({
          title: '请输入有效的手机号码',
          icon: 'none'
        })
        return
      }

      // 提交表单
      uni.showToast({
        title: '咨询已提交，我们会尽快联系您',
        icon: 'success',
        duration: 2000
      })

      // 清空表单
      setTimeout(() => {
        this.selectedProduct = null
        this.consultForm = {
          name: '',
          phone: '',
          email: '',
          colorIndex: 0,
          remarks: ''
        }
      }, 1500)
    }
  }
}
</script>

<style lang="scss" scoped>
.page {
  min-height: 100vh;
  background: #ffffff;
  padding-bottom: 120rpx;
  position: relative;
}

/* 自定义导航栏 */
.custom-navbar {
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: #ffffff;
  padding-top: constant(safe-area-inset-top);
  padding-top: env(safe-area-inset-top);
  box-shadow: 0 12rpx 36rpx rgba(0, 0, 0, 0.04);

  .navbar-content {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 16rpx 32rpx 76rpx;
    min-height: 160rpx;
    gap: 20rpx;
    position: relative;
  }

  .nav-pill-left {
    position: absolute;
    left: 32rpx;
    bottom: 12rpx;
  }

  .nav-title {
    flex: 1;
    text-align: center;
    font-size: 36rpx;
    color: #000000;
    font-weight: 600;
    letter-spacing: 1rpx;
    min-width: 0;
    white-space: nowrap;
    margin-bottom: 22rpx;
  }

  .nav-pill {
    display: flex;
    align-items: center;
    background: #ffffff;
    border: 1rpx solid rgba(0, 0, 0, 0.12);
    border-radius: 999rpx;
    padding: 0 20rpx;
    height: 56rpx;
    box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.06);
    flex-shrink: 0;
  }

  .nav-icon-button {
    width: 52rpx;
    height: 52rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: background 0.2s, opacity 0.2s;

    &:active {
      background: rgba(0, 0, 0, 0.06);
    }
  }

  .nav-divider {
    width: 2rpx;
    height: 36rpx;
    background: rgba(0, 0, 0, 0.12);
    margin: 0 16rpx;
  }

  .icon-arrow-left {
    width: 48rpx;
    height: 48rpx;
    display: block;
  }

  .icon-home {
    width: 48rpx;
    height: 48rpx;
    display: block;
  }
}

/* 页面头部 */
.header-section {
  padding: 60rpx 40rpx 40rpx;
  text-align: center;

  .title {
    display: block;
    font-size: 48rpx;
    font-weight: 600;
    color: #000000;
    margin-bottom: 16rpx;
    letter-spacing: 1rpx;
  }

  .subtitle {
    display: block;
    font-size: 28rpx;
    color: #666666;
    line-height: 1.5;
  }
}

/* 分类标签 */
.category-tabs {
  display: flex;
  gap: 16rpx;
  padding: 0 40rpx 24rpx;
  overflow-x: auto;
  scroll-behavior: smooth;

  &::-webkit-scrollbar {
    display: none;
  }

  .category-tab {
    flex-shrink: 0;
    padding: 12rpx 24rpx;
    background: #f5f5f5;
    border-radius: 24rpx;
    font-size: 26rpx;
    color: #666666;
    cursor: pointer;

    &.active {
      background: #000000;
      color: #ffffff;
      font-weight: 600;
    }
  }
}

/* 产品网格 */
.products-container {
  padding: 0 20rpx;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20rpx;
}

.product-card {
  background: #ffffff;
  border-radius: 12rpx;
  overflow: hidden;
  cursor: pointer;

  &:active {
    opacity: 0.9;
  }

  .product-image-wrapper {
    position: relative;
    width: 100%;
    aspect-ratio: 1;
    background: #f5f5f5;
    overflow: hidden;

    .product-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .badge {
      position: absolute;
      top: 12rpx;
      right: 12rpx;
      padding: 6rpx 12rpx;
      background: #000000;
      color: #ffffff;
      font-size: 20rpx;
      font-weight: 600;
      border-radius: 4rpx;
    }

    .favorite-icon {
      position: absolute;
      top: 12rpx;
      left: 12rpx;
      font-size: 32rpx;
      cursor: pointer;
      z-index: 5;
    }
  }

  .product-info {
    padding: 16rpx;
    display: flex;
    flex-direction: column;
    gap: 8rpx;

    .product-name {
      display: block;
      font-size: 26rpx;
      color: #333333;
      font-weight: 500;
      line-height: 1.3;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
    }

    .product-color {
      display: block;
      font-size: 20rpx;
      color: #999999;
    }

    .product-price {
      display: block;
      font-size: 28rpx;
      color: #000000;
      font-weight: 600;
    }
  }

  .color-dots {
    padding: 0 16rpx 16rpx;
    display: flex;
    gap: 8rpx;

    .color-dot {
      width: 24rpx;
      height: 24rpx;
      border-radius: 50%;
      border: 2rpx solid #e0e0e0;
      cursor: pointer;
    }
  }
}

/* 空状态 */
.empty-state {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 400rpx;

  .empty-text {
    font-size: 28rpx;
    color: #999999;
  }
}

/* 咨询表单（浮窗） */
.consultation-form {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #ffffff;
  border-radius: 24rpx 24rpx 0 0;
  box-shadow: 0 -4rpx 24rpx rgba(0, 0, 0, 0.1);
  z-index: 1000;
  max-height: 90vh;
  overflow-y: auto;
  animation: slideUp 0.3s ease-out;

  .form-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 24rpx 40rpx;
    border-bottom: 1px solid #f0f0f0;
    position: sticky;
    top: 0;
    background: #ffffff;

    .form-title {
      font-size: 32rpx;
      font-weight: 600;
      color: #000000;
    }

    .close-btn {
      font-size: 40rpx;
      color: #999999;
      cursor: pointer;
    }
  }

  .form-content {
    padding: 24rpx 40rpx 80rpx;

    .product-preview {
      text-align: center;
      margin-bottom: 32rpx;
      padding: 20rpx;
      background: #f9f9f9;
      border-radius: 8rpx;

      image {
        width: 100%;
        max-height: 240rpx;
        object-fit: contain;
        margin-bottom: 12rpx;
      }

      .preview-name {
        display: block;
        font-size: 24rpx;
        color: #333333;
        font-weight: 500;
      }
    }

    .form-group {
      margin-bottom: 24rpx;

      .label {
        display: block;
        font-size: 26rpx;
        color: #333333;
        font-weight: 500;
        margin-bottom: 12rpx;
      }

      input,
      textarea {
        width: 100%;
        padding: 16rpx;
        font-size: 26rpx;
        border: 1px solid #e0e0e0;
        border-radius: 8rpx;
        color: #333333;
        background: #ffffff;

        &::placeholder {
          color: #cccccc;
        }

        &:focus {
          border-color: #000000;
          outline: none;
        }
      }

      textarea {
        min-height: 120rpx;
        resize: none;
      }

      .char-count {
        display: block;
        text-align: right;
        font-size: 20rpx;
        color: #999999;
        margin-top: 8rpx;
      }

      .picker-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 16rpx;
        border: 1px solid #e0e0e0;
        border-radius: 8rpx;
        background: #ffffff;

        .picker-value {
          font-size: 26rpx;
          color: #333333;
        }

        .picker-arrow {
          font-size: 32rpx;
          color: #999999;
        }
      }
    }

    .form-actions {
      display: flex;
      gap: 16rpx;
      margin-top: 32rpx;

      .cancel-btn,
      .submit-btn {
        flex: 1;
        padding: 20rpx;
        border-radius: 8rpx;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 28rpx;
        font-weight: 600;
        cursor: pointer;

        &:active {
          opacity: 0.9;
        }
      }

      .cancel-btn {
        background: #f5f5f5;
        color: #333333;
      }

      .submit-btn {
        background: #000000;
        color: #ffffff;
      }
    }
  }
}

/* 遮罩层 */
.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  z-index: 999;
}

@keyframes slideUp {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}
</style>
