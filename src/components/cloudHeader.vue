<template>
  <view>
    <view
      class="header"
      :style="{
        background: background,
        height: height + 'px',
        'padding-top': top + 'px',
        zIndex: zIndex,
      }"
    >
      <view class="header-main">
        <slot></slot>
      </view>
      <view :style="{ width: BoundingWidth, height: '10px' }"></view>
    </view>
    <!-- 占位 view -->
    <view :style="{ height: viewHeight + 'px' }" v-if="isSeize"></view>
  </view>
</template>
<script>

export default {
  props: {
    background: {
      type: String,
      default: "",
    },
    height: {
      type: Number,
      default: 44,
    },
    haveStatusBar: {
      type: Boolean,
      default: true,
    },
    zIndex: {
      type: Number,
      default: 99,
    },
    isSeize: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      top: 0,
      viewHeight: 0,
      BoundingWidth: "0px",
    };
  },
  created: function () {
    // #ifndef MP-ALIPAY
    var res = uni.getSystemInfoSync();
    this.top = res.statusBarHeight;
    this.viewHeight = this.top + uni.upx2px(this.height);
    // #ifdef MP-WEIXIN
    // 小程序胶囊按钮
    var res = uni.getMenuButtonBoundingClientRect();
    this.BoundingWidth = res.width + 20 + "px";
    // #endif
    // #endif
  },
};
</script>
<style scoped>
.header {
  width: 100%;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99;
  height: 44px;
  padding-top: 20px;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.header-main {
  width: 300rpx;
  flex: auto;
}
</style>
