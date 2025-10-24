<template>
  <view class="page">
    <!-- 自定义顶部导航栏 -->
    <view class="custom-navbar">
      <view class="navbar-content">
        <text class="brand-logo">RUIZHU</text>
      </view>
    </view>

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
        @change="onBannerChange"
      >
        <!-- 第一个轮播项：WebP 动画 -->
        <swiper-item>
          <view class="banner-item video-item">
            <image class="banner-image" src="https://ompeak.com/banner-animation.webp" mode="aspectFill"></image>
          </view>
        </swiper-item>

        <!-- 其他轮播项 -->
        <swiper-item v-for="(item, index) in bannerList" :key="index">
          <view class="banner-item">
            <image class="banner-image" :src="item.image" mode="aspectFill"></image>
            <view class="banner-overlay">
              <text class="banner-title">{{ item.title }}</text>
              <view class="banner-subtitle">
                <text class="subtitle-text">{{ item.subtitle }}</text>
                <view class="subtitle-line"></view>
              </view>
            </view>
          </view>
        </swiper-item>
      </swiper>
    </view>

    <!-- 页面标题 -->
    <view class="page-header">
      <text class="main-title">VIP私人定制</text>
      <text class="sub-title">专属尊贵体验</text>
    </view>

    <!-- 分类标签 -->
    <view class="category-tabs">
      <view
        class="tab-item"
        :class="{ active: activeTab === 'women' }"
        @tap="switchTab('women')"
      >
        <text class="tab-text">女士甄选</text>
        <view v-if="activeTab === 'women'" class="tab-line"></view>
      </view>
      <view
        class="tab-item"
        :class="{ active: activeTab === 'men' }"
        @tap="switchTab('men')"
      >
        <text class="tab-text">男士甄选</text>
        <view v-if="activeTab === 'men'" class="tab-line"></view>
      </view>
    </view>

    <!-- 产品展示 Swiper 区域 -->
    <swiper
      class="products-swiper"
      :indicator-dots="false"
      :autoplay="false"
      :circular="true"
      @change="onSwiperChange"
    >
      <swiper-item v-for="(slide, slideIndex) in productSlides" :key="slideIndex">
        <view class="products-section">
          <!-- 左侧大图展示 -->
          <view class="featured-product" @tap="onProductTap(slide.featured)">
            <image
              class="featured-image"
              :src="slide.featured.image"
              mode="aspectFill"
            ></image>
            <view class="featured-info">
              <text class="featured-name">{{ slide.featured.name }}</text>
              <text class="featured-price">¥ {{ slide.featured.price }}</text>
            </view>
          </view>

          <!-- 右侧产品列表 -->
          <view class="products-list">
            <view
              class="product-card"
              v-for="(product, index) in slide.products"
              :key="index"
              @tap="onProductTap(product)"
            >
              <image
                class="product-image"
                :src="product.image"
                mode="aspectFill"
              ></image>
              <view class="product-info">
                <text class="product-name">{{ product.name }}</text>
                <text class="product-price">¥ {{ product.price }}</text>
              </view>
            </view>
          </view>
        </view>
      </swiper-item>
    </swiper>

    <!-- 探索更多按钮 -->
    <view class="explore-more" @tap="onExploreMore">
      <text class="explore-text">探索更多</text>
    </view>

    <!-- 轮播指示器 -->
    <view class="indicator-dots">
      <view
        class="dot"
        v-for="(dot, slideIndex) in productSlides.length"
        :key="slideIndex"
        :class="{ active: slideIndex === currentSlide }"
      ></view>
    </view>

    <!-- 更多分类区域 -->
    <view class="more-categories">
      <text class="category-title">定制系列</text>

      <view class="category-grid">
        <view
          class="category-item"
          v-for="(category, index) in categories"
          :key="index"
          @tap="onCategoryTap(category)"
        >
          <image
            class="category-image"
            :src="category.image"
            mode="aspectFill"
          ></image>
          <view class="category-overlay">
            <text class="category-name">{{ category.name }}</text>
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
      // Banner 相关
      indicatorColor: 'rgba(255, 255, 255, 0.5)',
      indicatorActiveColor: '#ffffff',
      currentBannerIndex: 0,
      bannerList: [
        {
          title: 'VIP定制系列',
          subtitle: '尊贵定制',
          image: 'https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800&q=80'
        },
        {
          title: '私人定制',
          subtitle: '专属服务',
          image: 'https://images.unsplash.com/photo-1483985988355-763728e1935b?w=800&q=80'
        },
        {
          title: '尊享礼遇',
          subtitle: '至尊体验',
          image: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?w=800&q=80'
        }
      ],

      // 产品相关
      activeTab: 'women',
      currentSlide: 0,
      productSlides: [
        {
          featured: {
            name: '定制时尚鞋',
            price: '1999',
            image: '/static/images/product/120251017222234.jpg'
          },
          products: [
            {
              name: '高级配饰',
              price: '12,500',
              image: '/static/images/product/120251017184152.jpg'
            },
            {
              name: '高级配饰',
              price: '5,800',
              image: '/static/images/product/120251017184157.jpg'
            }
          ]
        },
        {
          featured: {
            name: '奢华商务包',
            price: '35,600',
            image: '/static/images/product/120251017222229.jpg'
          },
          products: [
            {
              name: '高级配饰',
              price: '8,900',
              image: '/static/images/product/120251017184201.jpg'
            },
            {
              name: '高级配饰',
              price: '15,800',
              image: '/static/images/product/120251017184205.jpg'
            }
          ]
        },
        {
          featured: {
            name: '时尚背包系列',
            price: '22,500',
            image: '/static/images/product/120251017222238.jpg'
          },
          products: [
            {
              name: '高级配饰',
              price: '18,900',
              image: '/static/images/product/120251017184212.jpg'
            },
            {
              name: '高级配饰',
              price: '9,200',
              image: '/static/images/product/120251017184216.jpg'
            }
          ]
        }
      ],
      categories: [
        {
          name: '高级配饰',
          image: '/static/images/product/120251017184219.jpg'
        },
        {
          name: '高级配饰',
          image: '/static/images/product/120251017184152.jpg'
        },
        {
          name: '定制鞋履',
          image: '/static/images/product/120251017222234.jpg'
        },
        {
          name: '高级配饰',
          image: '/static/images/product/120251017184201.jpg'
        }
      ]
    }
  },
  methods: {
    onBannerChange(e) {
      this.currentBannerIndex = e.detail.current
    },
    switchTab(tab) {
      this.activeTab = tab
      // 这里可以加载不同的产品数据
      console.log('切换到:', tab === 'women' ? '女士' : '男士')
    },
    onSwiperChange(e) {
      this.currentSlide = e.detail.current
    },
    onProductTap(product) {
      uni.showToast({
        title: product.name,
        icon: 'none'
      })
    },
    onExploreMore() {
      uni.showToast({
        title: '探索更多产品',
        icon: 'none'
      })
    },
    onCategoryTap(category) {
      uni.showToast({
        title: category.name,
        icon: 'none'
      })
    }
  }
}
</script>

<style lang="scss">
.page {
  min-height: 100vh;
  background: #ffffff;
  padding-bottom: 120rpx;
}

/* 自定义导航栏 */
.custom-navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: #ffffff;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  box-shadow: 0 1rpx 3rpx rgba(0, 0, 0, 0.06);
  padding-top: constant(safe-area-inset-top);
  padding-top: env(safe-area-inset-top);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  .navbar-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20rpx 40rpx;
    height: 88rpx;
  }

  .brand-logo {
    font-size: 48rpx;
    font-weight: 700;
    letter-spacing: 4rpx;
    color: #000000;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    transition: all 0.2s ease;
    flex: 1;
  }
}

/* 轮播图区域 */
.banner-section {
  margin-top: calc(88rpx + constant(safe-area-inset-top));
  margin-top: calc(88rpx + env(safe-area-inset-top));
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

  .banner-overlay {
    position: absolute;
    bottom: 100rpx;
    left: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 2;
  }

  .banner-title {
    font-size: 64rpx;
    font-weight: 500;
    color: #ffffff;
    letter-spacing: 2rpx;
    margin-bottom: 20rpx;
    text-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.3);
  }

  .banner-subtitle {
    display: flex;
    flex-direction: column;
    align-items: center;

    .subtitle-text {
      font-size: 28rpx;
      color: #ffffff;
      letter-spacing: 1rpx;
      margin-bottom: 12rpx;
    }

    .subtitle-line {
      width: 120rpx;
      height: 2rpx;
      background: #ffffff;
    }
  }

  /* 视频项特殊样式 */
  .video-item {
    background: #000000;
  }
}

/* 页面标题 */
.page-header {
  padding: 40rpx 0 60rpx;
  text-align: center;

  .main-title {
    display: block;
    font-size: 56rpx;
    font-weight: 500;
    color: #000000;
    letter-spacing: 2rpx;
    margin-bottom: 20rpx;
  }

  .sub-title {
    display: block;
    font-size: 28rpx;
    color: #666666;
    letter-spacing: 1rpx;
  }
}

/* 分类标签 */
.category-tabs {
  display: flex;
  justify-content: center;
  gap: 80rpx;
  padding: 0 40rpx 60rpx;

  .tab-item {
    position: relative;
    padding-bottom: 20rpx;
    cursor: pointer;

    .tab-text {
      font-size: 32rpx;
      color: #999999;
      transition: all 0.3s;
    }

    &.active .tab-text {
      color: #000000;
      font-weight: 500;
    }

    .tab-line {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      height: 4rpx;
      background: #000000;
    }
  }
}

/* 产品 Swiper */
.products-swiper {
  height: 760rpx;
  margin-bottom: 60rpx;
}

/* 产品展示区域 */
.products-section {
  display: flex;
  gap: 24rpx;
  padding: 0 40rpx;
  height: 100%;
  align-items: flex-start;

  .featured-product {
    flex: 1;
    background: #f5f5f5;
    border-radius: 8rpx;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    max-height: 100%;

    .featured-image {
      width: 100%;
      height: 520rpx;
      flex-shrink: 0;
      object-fit: cover;
    }

    .featured-info {
      padding: 24rpx;
      flex: 0;

      .featured-name {
        display: block;
        font-size: 28rpx;
        color: #333333;
        margin-bottom: 12rpx;
      }

      .featured-price {
        display: block;
        font-size: 32rpx;
        color: #000000;
        font-weight: 600;
      }
    }
  }

  .products-list {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 16rpx;
    max-height: 100%;
    overflow: hidden;

    .product-card {
      background: #f5f5f5;
      border-radius: 8rpx;
      overflow: hidden;
      flex: 1;
      display: flex;
      flex-direction: column;
      min-height: 0;

      .product-image {
        width: 100%;
        height: 220rpx;
        flex-shrink: 0;
        object-fit: cover;
      }

      .product-info {
        padding: 16rpx;
        flex: 0;

        .product-name {
          display: block;
          font-size: 26rpx;
          color: #333333;
          margin-bottom: 8rpx;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }

        .product-price {
          display: block;
          font-size: 28rpx;
          color: #000000;
          font-weight: 600;
        }
      }
    }
  }
}

/* 探索更多按钮 */
.explore-more {
  margin: 0 40rpx 40rpx;
  padding: 32rpx 0;
  background: #000000;
  border-radius: 8rpx;
  text-align: center;

  .explore-text {
    font-size: 32rpx;
    color: #ffffff;
    font-weight: 500;
    letter-spacing: 1rpx;
  }
}

/* 轮播指示器 */
.indicator-dots {
  display: flex;
  justify-content: center;
  gap: 16rpx;
  padding: 40rpx 0;

  .dot {
    width: 16rpx;
    height: 16rpx;
    border-radius: 50%;
    background: #d8d8d8;
    transition: all 0.3s;

    &.active {
      background: #000000;
      width: 32rpx;
      border-radius: 8rpx;
    }
  }
}

/* 更多分类区域 */
.more-categories {
  padding: 60rpx 40rpx;

  .category-title {
    display: block;
    font-size: 48rpx;
    font-weight: 500;
    color: #000000;
    text-align: center;
    margin-bottom: 60rpx;
    letter-spacing: 2rpx;
  }

  .category-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24rpx;

    .category-item {
      position: relative;
      height: 340rpx;
      border-radius: 8rpx;
      overflow: hidden;
      display: flex;
      flex-direction: column;

      .category-image {
        width: 100%;
        height: 100%;
        flex: 1;
        object-fit: cover;
      }

      .category-overlay {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 30rpx;
        background: linear-gradient(to top, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.3), transparent);
        min-height: 120rpx;
        display: flex;
        align-items: flex-end;
        z-index: 2;

        .category-name {
          display: block;
          font-size: 32rpx;
          color: #ffffff;
          font-weight: 500;
          text-align: center;
          width: 100%;
          text-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.5);
        }
      }
    }
  }
}
</style>
