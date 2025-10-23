<script>
export default {
  onLaunch: function() {
    console.log('App Launch')
  },
  onShow: function() {
    console.log('App Show')
  },
  onHide: function() {
    console.log('App Hide')
  }
}
</script>

<style lang="scss">
@import '@/uni.scss';

page {
  background-color: #ffffff;
  transition: background-color 0.3s ease;
}

// 全局按钮样式优化
button {
  transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1) !important;

  &:active {
    transform: scale(0.95);
  }
}

// 全局view点击反馈
view[onclick],
view[data-tap-index],
[data-clickable="true"] {
  transition: all 0.2s ease;

  &:active {
    opacity: 0.8;
  }
}

// 文本输入框优化
input {
  transition: all 0.2s ease;

  &:focus {
    box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.1);
  }
}

// 页面进入动画
page {
  animation: pageIn 0.3s ease forwards;
}

@keyframes pageIn {
  from {
    opacity: 0;
    transform: translateY(10rpx);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
