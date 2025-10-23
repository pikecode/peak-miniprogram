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
        @change="onSwiperChange"
      >
        <!-- 第一个轮播项：WebP 动画 -->
        <swiper-item>
          <view class="banner-item video-item">
            <image class="banner-image" src="/static/animation.webp" mode="aspectFill"></image>
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

    <!-- 会员礼遇（参考视觉模块） -->
    <view class="benefits-section">
      <text class="section-title">会员礼遇</text>
      <view class="benefits-card">
        <view class="benefit-items">
          <!-- 左侧礼遇 -->
          <view class="benefit-item">
            <view class="benefit-media icon">
              <image class="benefit-image" src="/static/images/logo.jpg" mode="aspectFit"></image>
            </view>
            <text class="benefit-title">至高尊享12期免息</text>
            <text class="benefit-desc">单笔订单金额≥15,000元可享</text>
          </view>

          <!-- 右侧礼遇 -->
          <view class="benefit-item">
            <view class="benefit-media">
              <image class="benefit-image" src="/static/images/product/120251017184208.jpg" mode="aspectFit"></image>
            </view>
            <text class="benefit-title">品牌定制笔记本</text>
            <view class="benefit-desc-wrap">
              <text class="benefit-desc">会员单笔订单≥15,000元加赠</text>
              <text class="benefit-desc muted">（送完为止）</text>
            </view>
          </view>
        </view>

        <view class="benefit-action" @tap="onJoinNow">
          <text>即刻入会</text>
        </view>
      </view>
    </view>

    <!-- 陈列货架模块（两列×两行 + 探索更多） -->
    <view class="collection-section">
      <!-- 第1行 -->
      <view class="shelf-row">
        <view class="shelf-items">
          <view
            v-for="(p, i) in shelfProducts.slice(0, 2)"
            :key="'s1-' + i"
            class="shelf-item"
            @tap="onShelfProductTap(p)"
          >
            <image class="shelf-image" :src="p.image" mode="aspectFit"></image>
            <view class="shelf-meta">
              <text class="shelf-en">{{ p.en }}</text>
              <text class="shelf-cn">{{ p.cn }}</text>
              <text class="shelf-price">¥ {{ p.price }}</text>
            </view>
          </view>
        </view>
        <view class="shelf-bar"></view>
      </view>

      <!-- 第2行 -->
      <view class="shelf-row">
        <view class="shelf-items">
          <view
            v-for="(p, i) in shelfProducts.slice(2, 4)"
            :key="'s2-' + i"
            class="shelf-item"
            @tap="onShelfProductTap(p)"
          >
            <image class="shelf-image" :src="p.image" mode="aspectFit"></image>
            <view class="shelf-meta">
              <text class="shelf-en">{{ p.en }}</text>
              <text class="shelf-cn">{{ p.cn }}</text>
              <text class="shelf-price">¥ {{ p.price }}</text>
            </view>
          </view>
        </view>
        <view class="shelf-bar"></view>
      </view>

      <view class="collection-action" @tap="onExploreMoreShelves">
        <text>探索更多</text>
      </view>
    </view>

    <!-- 会员特权区域 -->
    <GridSection
      title="会员特权"
      :items="memberCards"
      cardType="member-card"
      @item-tap="onCardTap"
    />

    <!-- 精选推荐区域 -->
    <GridSection
      title="精选推荐"
      :items="products"
      cardType="product-card"
      @item-tap="onProductTap"
    />

    <!-- 推荐商品区域（3列） -->
    <GridSection
      title="推荐商品"
      :items="recommendProducts"
      cardType="recommend-card"
      :columns="3"
      @item-tap="onProductTap"
    />
  </view>
</template>

<script>
import GridSection from '@/components/GridSection.vue'

export default {
  components: {
    GridSection
  },
  data() {
    return {
      // 视频相关
      videoUrl: '/static/video.mp4',
      showPlayBtn: true,

      indicatorColor: 'rgba(255, 255, 255, 0.5)',
      indicatorActiveColor: '#ffffff',
      currentBannerIndex: 0,
      bannerList: [
        {
          title: 'Ruizhu Collection',
          subtitle: '即刻探索',
          image: 'https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800&q=80'
        },
        {
          title: '新品上市',
          subtitle: '限时优惠',
          image: 'https://images.unsplash.com/photo-1483985988355-763728e1935b?w=800&q=80'
        },
        {
          title: '经典系列',
          subtitle: '永恒之选',
          image: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?w=800&q=80'
        }
      ],
      memberCards: [
        {
          label: '会员专享',
          image: 'https://images.unsplash.com/photo-1607082348824-0a96f2a4b9da?w=400&q=80'
        },
        {
          label: '积分商城',
          image: 'https://images.unsplash.com/photo-1607082349566-187342175e2f?w=400&q=80'
        }
      ],
      products: [
        {
          name: '经典手袋',
          price: '12800',
          image: 'https://images.unsplash.com/photo-1584917865442-de89df76afd3?w=400&q=80'
        },
        {
          name: '时尚背包',
          price: '8600',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80'
        },
        {
          name: '优雅钱包',
          price: '3200',
          image: 'https://images.unsplash.com/photo-1627123424574-724758594e93?w=400&q=80'
        },
        {
          name: '商务公文包',
          price: '15800',
          image: 'https://images.unsplash.com/photo-1590874103328-eac38a683ce7?w=400&q=80'
        }
      ],

      // 首页：陈列货架模块（两行 × 两列）
      shelfProducts: [
        {
          en: 'Re-Nylon',
          cn: '双肩背包',
          price: '21,800',
          image: '/static/images/product/120251017222229.jpg'
        },
        {
          en: 'Re-Nylon',
          cn: '双肩背包',
          price: '21,800',
          image: '/static/images/product/120251017222238.jpg'
        },
        {
          en: 'Re-Nylon与牛皮革',
          cn: '拼接双肩背包',
          price: '28,700',
          image: '/static/images/product/120251017222242.jpg'
        },
        {
          en: 'Re-Nylon与牛皮革',
          cn: '拼接双肩背包',
          price: '28,700',
          image: '/static/images/product/120251017222234.jpg'
        }
      ],
      // 推荐商品（3列）
      recommendProducts: [
        {
          name: '【明星同款】Prada Explore 中号Re-...',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80'
        },
        {
          name: '【预售】Prada Explore中号Nappa...',
          image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=400&q=80'
        },
        {
          name: 'Re-Nylon双肩背包',
          image: 'https://images.unsplash.com/photo-1556821552-5f06b5991ce0?w=400&q=80'
        },
        {
          name: '【预售】皮革中筒靴',
          image: 'https://images.unsplash.com/photo-1543163521-1bf539c55dd2?w=400&q=80'
        },
        {
          name: 'Prada Bonnie 迷你牛皮革手袋',
          image: 'https://images.unsplash.com/photo-1548062407-f961713e6786?w=400&q=80'
        },
        {
          name: 'Prada Re-Edition 1978小号Re-Nylon...',
          image: 'https://images.unsplash.com/photo-1627123424574-724758594e93?w=400&q=80'
        },
        {
          name: 'Re-Nylon双肩背包',
          image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80'
        },
        {
          name: '再生尼龙羽绒夹克',
          image: 'https://images.unsplash.com/photo-1551028719-00167b16ebc5?w=400&q=80'
        },
        {
          name: '亮面皮革乐福鞋',
          image: 'https://images.unsplash.com/photo-1509631179647-0177331693ae?w=400&q=80'
        }
      ]
    }
  },
  onLoad() {
    console.log('Ruizhu 首页加载完成')
  },
  methods: {
    onJoinNow() {
      // 跳转至入会资料完善页
      uni.navigateTo({
        url: '/pages/membership/join',
        fail: () => {
          uni.showToast({ title: '页面开发中', icon: 'none' })
        }
      })
    },
    onShelfProductTap(p) {
      uni.showToast({ title: `${p.en} ${p.cn}`, icon: 'none' })
    },
    onExploreMoreShelves() {
      // 跳转到系列探索页
      uni.navigateTo({ url: '/pages/collection/explore' })
    },
    onVideoImageTap() {
      // 点击视频封面，可以打开视频播放器或跳转到视频详情
      uni.navigateTo({
        url: '/pages/video-player/video-player',  // 需要创建此页面
        fail: () => {
          // 如果页面不存在，显示提示
          uni.showToast({
            title: '视频播放器开发中',
            icon: 'none'
          })
        }
      })
    },

    onSwiperChange(e) {
      this.currentBannerIndex = e.detail.current
    },
    onCardTap(card) {
      uni.showToast({
        title: card.label,
        icon: 'none'
      })
    },
    onProductTap(product) {
      uni.showToast({
        title: product.name,
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

  .navbar-actions {
    display: flex;
    align-items: center;
    gap: 32rpx;
    flex-shrink: 0;

    text {
      font-size: 44rpx;
      color: #000000;
      font-weight: 300;
      transition: all 0.2s ease;
      cursor: pointer;

      &:active {
        color: #666666;
        transform: scale(0.9);
      }
    }
  }
}

/* 会员礼遇（参考视觉模块） */
.benefits-section {
  padding: 60rpx 40rpx 0;

  .section-title {
    display: block;
    font-size: 48rpx;
    font-weight: 500;
    color: #000000;
    text-align: center;
    margin-bottom: 30rpx;
    letter-spacing: 2rpx;
  }

  .benefits-card {
    background: #f4f4f4;
    border-radius: 8rpx;
    padding: 40rpx 24rpx 32rpx;
  }

  .benefit-items {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24rpx;
    align-items: start;
    margin-bottom: 28rpx;
  }

  .benefit-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding: 8rpx 8rpx 0;
  }

  .benefit-media {
    width: 220rpx;
    height: 160rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 12rpx;
  }

  .benefit-media.icon {
    border-radius: 8rpx;
  }

  .benefit-image {
    width: 100%;
    height: 100%;
  }

  .benefit-title {
    display: block;
    font-size: 28rpx;
    color: #000000;
    font-weight: 600;
    margin-bottom: 6rpx;
  }

  .benefit-desc-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4rpx;
  }

  .benefit-desc {
    display: block;
    font-size: 24rpx;
    color: #666666;
  }

  .benefit-desc.muted {
    color: #9a9a9a;
  }

  .benefit-action {
    height: 72rpx;
    width: 300rpx;
    margin: 0 auto;
    background: #000000;
    color: #ffffff;
    border-radius: 6rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 28rpx;
    font-weight: 600;
    transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;

    &:active {
      background: #333333;
      transform: scale(0.95);
    }
  }
}

/* 陈列货架模块 */
.collection-section {
  margin-top: 40rpx;
  padding: 40rpx 20rpx 20rpx;
  background: linear-gradient(180deg, #f6f2eb 0%, #f9f7f2 100%);

  .shelf-row {
    position: relative;
    padding: 24rpx 20rpx 60rpx;
    margin-bottom: 20rpx;
  }

  .shelf-items {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20rpx;
    align-items: end;
  }

  .shelf-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 16rpx;
  }

  .shelf-image {
    width: 260rpx;
    height: 240rpx;
  }

  .shelf-meta {
    display: flex;
    flex-direction: column;
    gap: 6rpx;
  }
  .shelf-en { font-size: 28rpx; color: #000; font-weight: 600; }
  .shelf-cn { font-size: 26rpx; color: #333; }
  .shelf-price { font-size: 26rpx; color: #000; font-weight: 600; }

  .shelf-bar {
    position: absolute;
    left: 30rpx; right: 30rpx; bottom: 24rpx;
    height: 18rpx;
    background: linear-gradient(180deg, #e2c496 0%, #d1ae79 100%);
    border-radius: 6rpx;
    box-shadow: 0 8rpx 18rpx rgba(0, 0, 0, 0.12);
  }

  .collection-action {
    margin: 12rpx auto 10rpx;
    width: 320rpx; height: 88rpx;
    border-radius: 8rpx;
    background: #000; color: #fff;
    display: flex; align-items: center; justify-content: center;
    font-size: 32rpx; font-weight: 600;
    transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;

    &:active {
      background: #333;
      transform: scale(0.95);
    }
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

</style>
