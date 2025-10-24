<template>
  <view class="edit-profile-page">
    <!-- 表单内容 -->
    <scroll-view class="form-container" scroll-y="true">
      <!-- 个人信息部分 -->
      <view class="form-section">
        <text class="section-title">个人信息</text>

        <!-- 称谓 -->
        <view class="form-group">
          <view class="form-control">
            <picker :range="['先生', '女士']" @change="onSalutationChange" class="picker-wrapper">
              <view class="picker-content">
                <view class="picker-value">{{ formData.salutation }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
          </view>
        </view>

        <!-- 姓 -->
        <view class="form-group">
          <view class="form-control">
            <input
              class="form-input"
              v-model="formData.lastName"
              placeholder="输入姓氏"
              placeholder-class="input-placeholder"
            />
          </view>
        </view>

        <!-- 名 -->
        <view class="form-group">
          <view class="form-control">
            <input
              class="form-input"
              v-model="formData.firstName"
              placeholder="输入名字"
              placeholder-class="input-placeholder"
            />
          </view>
        </view>

        <!-- 电话 -->
        <view class="form-group">
          <view class="form-control">
            <text class="phone-label">微信授权手机号码</text>
          </view>
        </view>

        <!-- 出生日期 -->
        <view class="form-group">
          <view class="form-control date-group">
            <picker :range="yearRange" @change="onYearChange" class="date-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.year }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
            <picker :range="monthRange" @change="onMonthChange" class="date-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.month }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
            <picker :range="dayRange" @change="onDayChange" class="date-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.day }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
          </view>
        </view>

        <!-- 省份 -->
        <view class="form-group">
          <view class="form-control location-group">
            <picker :range="provinces" @change="onProvinceChange" class="location-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.province }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
            <picker :range="cities" @change="onCityChange" class="location-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.city }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
            <picker :range="districts" @change="onDistrictChange" class="location-picker">
              <view class="picker-content">
                <view class="picker-value">{{ formData.district }}</view>
                <text class="picker-arrow">›</text>
              </view>
            </picker>
          </view>
        </view>
      </view>

      <!-- 授权复选框 -->
      <view class="checkbox-group">
        <!-- 第一个复选框 -->
        <view class="checkbox-item">
          <view class="checkbox-wrapper">
            <view class="checkbox" :class="{ checked: formData.agree1 }" @tap="toggleCheckbox1">
              <text v-if="formData.agree1" class="checkbox-icon">✔</text>
            </view>
          </view>
          <view class="checkbox-content">
            <view class="checkbox-text">
              <text class="highlight">*我希望在 RUIZHU 小程序上创建我的帐户，</text>
              <text>并且知悉我的个人信息将用于提供如</text>
              <text class="highlight">隐私政策</text>
              <text>中所述所要求的服务。</text>
            </view>
            <view class="checkbox-description">
              <text>用于创建您的在线帐户，您需要提供您的姓名、手机号码和位置。如果您拒绝将导致本服务服务无法正常运行，我们将无法为您提供服务。</text>
            </view>
          </view>
        </view>

        <!-- 第二个复选框 -->
        <view class="checkbox-item">
          <view class="checkbox-wrapper">
            <view class="checkbox" :class="{ checked: formData.agree2 }" @tap="toggleCheckbox2">
              <text v-if="formData.agree2" class="checkbox-icon">✔</text>
            </view>
          </view>
          <view class="checkbox-content">
            <view class="checkbox-text">
              <text>我亦希望加入睿珠集团顾客数据库，以便获得定制的全球客户服务，并享受为注册会员保留的专属服务和优惠。</text>
            </view>
            <view class="checkbox-description">
              <text>用于注册目的，您需要提供您的手机、号码。如果您拒绝将导致本服务服务无法正常运行，我们将无法为您提供服务。</text>
            </view>
          </view>
        </view>
      </view>

      <!-- 权限说明 -->
      <view class="permission-info">
        <text>我亦同意贵司为下列可选目的的收集、使用与披露我的个人信息：</text>
      </view>

      <!-- 保存按钮 -->
      <view class="button-group">
        <view class="save-btn" @tap="handleSave">
          <text>保存</text>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        salutation: '先生',
        lastName: '张',
        firstName: '',
        phone: '18621872825',
        year: '1990',
        month: '01',
        day: '01',
        province: '浙江省',
        city: '杭州',
        district: '西湖区',
        agree1: true,
        agree2: true
      },
      yearRange: Array.from({ length: 100 }, (_, i) => (new Date().getFullYear() - 99 + i).toString()),
      monthRange: Array.from({ length: 12 }, (_, i) => String(i + 1).padStart(2, '0')),
      dayRange: Array.from({ length: 31 }, (_, i) => String(i + 1).padStart(2, '0')),
      provinces: ['浙江省', '北京市', '上海市', '广东省', '江苏省'],
      cities: ['杭州', '宁波', '温州', '嘉兴'],
      districts: ['西湖区', '上城区', '下城区', '江干区']
    }
  },
  onLoad() {
    console.log('编辑个人信息页面加载完成')
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    onSalutationChange(e) {
      const salutations = ['先生', '女士']
      this.formData.salutation = salutations[e.detail.value]
    },
    onYearChange(e) {
      this.formData.year = this.yearRange[e.detail.value]
    },
    onMonthChange(e) {
      this.formData.month = this.monthRange[e.detail.value]
    },
    onDayChange(e) {
      this.formData.day = this.dayRange[e.detail.value]
    },
    onProvinceChange(e) {
      this.formData.province = this.provinces[e.detail.value]
    },
    onCityChange(e) {
      this.formData.city = this.cities[e.detail.value]
    },
    onDistrictChange(e) {
      this.formData.district = this.districts[e.detail.value]
    },
    toggleCheckbox1() {
      this.formData.agree1 = !this.formData.agree1
    },
    toggleCheckbox2() {
      this.formData.agree2 = !this.formData.agree2
    },
    handleSave() {
      if (!this.formData.agree1) {
        uni.showToast({
          title: '请同意第一项条款',
          icon: 'none'
        })
        return
      }
      uni.showToast({
        title: '信息已保存',
        icon: 'none'
      })
      setTimeout(() => {
        uni.navigateBack()
      }, 1500)
    }
  }
}
</script>

<style lang="scss" scoped>
.edit-profile-page {
  height: 100vh;
  background: #ffffff;
  display: flex;
  flex-direction: column;
}

/* 表单容器 */
.form-container {
  flex: 1;
  overflow-y: auto;
  padding: 40rpx 24rpx;
  width: 100%;
  box-sizing: border-box;
}

/* 表单部分 */
.form-section {
  margin-bottom: 40rpx;

  .section-title {
    display: block;
    font-size: 32rpx;
    font-weight: 600;
    color: #000000;
    margin-bottom: 32rpx;
  }
}

/* 表单组 */
.form-group {
  margin-bottom: 40rpx;
  display: flex;
  flex-direction: column;
  width: 100%;

  .form-control {
    display: flex;
    align-items: center;
    border-bottom: 1px solid #e0e0e0;
    padding-bottom: 12rpx;
    min-width: 0;

    .picker-value {
      flex: 1;
      font-size: 28rpx;
      color: #000000;
    }

    .picker-arrow {
      font-size: 28rpx;
      color: #999999;
    }

    .phone-label {
      font-size: 28rpx;
      color: #999999;
    }
  }

  .form-input {
    flex: 1;
    font-size: 28rpx;
    color: #000000;
    width: 100%;
    padding: 0;
    margin: 0;
    background: transparent;
  }

  .input-placeholder {
    color: #cccccc;
    font-size: 28rpx;
  }

  .picker-wrapper {
    display: flex;
    flex: 1;
    width: 100%;

    .picker-content {
      display: flex;
      flex-direction: row;
      align-items: center;
      width: 100%;
      gap: 4rpx;

      .picker-value {
        flex: 1;
        font-size: 28rpx;
        color: #000000;
      }

      .picker-arrow {
        font-size: 28rpx;
        color: #999999;
        flex-shrink: 0;
      }
    }
  }
}

/* 日期选择组 */
.date-group {
  display: flex;
  gap: 12rpx;
  align-items: center;
  width: 100%;
  border-bottom: none !important;
  padding-bottom: 0 !important;
  flex-direction: row;

  .date-picker {
    flex: 1;
    display: flex;
    align-items: center;
    border-bottom: 1px solid #e0e0e0;
    padding-bottom: 12rpx;
    min-width: 0;

    .picker-content {
      display: flex;
      flex-direction: row;
      align-items: center;
      width: 100%;
      gap: 4rpx;

      .picker-value {
        flex: 1;
        font-size: 26rpx;
        color: #000000;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }

      .picker-arrow {
        font-size: 20rpx;
        color: #999999;
        flex-shrink: 0;
      }
    }
  }
}

/* 地区选择组 */
.location-group {
  display: flex;
  gap: 12rpx;
  align-items: center;
  width: 100%;
  border-bottom: none !important;
  padding-bottom: 0 !important;
  flex-direction: row;

  .location-picker {
    flex: 1;
    display: flex;
    align-items: center;
    border-bottom: 1px solid #e0e0e0;
    padding-bottom: 12rpx;
    min-width: 0;

    .picker-content {
      display: flex;
      flex-direction: row;
      align-items: center;
      width: 100%;
      gap: 4rpx;

      .picker-value {
        flex: 1;
        font-size: 26rpx;
        color: #000000;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }

      .picker-arrow {
        font-size: 20rpx;
        color: #999999;
        flex-shrink: 0;
      }
    }
  }
}

/* 复选框组 */
.checkbox-group {
  margin-top: 60rpx;
  margin-bottom: 40rpx;

  .checkbox-item {
    display: flex;
    gap: 16rpx;
    margin-bottom: 40rpx;

    .checkbox-wrapper {
      flex: 0 0 32rpx;
      padding-top: 8rpx;

      .checkbox {
        width: 32rpx;
        height: 32rpx;
        border: 2px solid #000000;
        border-radius: 4rpx;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.2s ease;

        &.checked {
          background: #000000;

          .checkbox-icon {
            color: #ffffff;
            font-size: 18rpx;
            font-weight: 600;
          }
        }

        .checkbox-icon {
          display: none;
        }
      }
    }

    .checkbox-content {
      flex: 1;

      .checkbox-text {
        font-size: 26rpx;
        line-height: 1.6;
        color: #000000;
        margin-bottom: 12rpx;

        .highlight {
          font-weight: 600;
        }
      }

      .checkbox-description {
        font-size: 22rpx;
        color: #999999;
        line-height: 1.6;
      }
    }
  }
}

/* 权限信息 */
.permission-info {
  font-size: 26rpx;
  color: #000000;
  line-height: 1.6;
  margin-bottom: 60rpx;
}

/* 按钮组 */
.button-group {
  margin-top: 40rpx;
  margin-bottom: 60rpx;

  .save-btn {
    width: 100%;
    height: 88rpx;
    background: #000000;
    color: #ffffff;
    font-size: 32rpx;
    font-weight: 600;
    border-radius: 8rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.2s ease;

    &:active {
      background: #333333;
      transform: scale(0.98);
    }
  }
}
</style>
