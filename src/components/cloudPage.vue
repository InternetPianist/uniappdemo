<template>
  <view class="sbody">
    <!-- 头部导航 -->
    <view v-if="customHeader">
      <view
        class="page-header"
        :style="{
          'z-index': headerIndex,
          background: headerBG,
          borderBottomWidth: borderWidth,
          borderBottomColor: borderColor
        }"
      >
        <!-- 沉浸式状态栏 -->
        <view
          class="page-status-bar"
          :style="{ height: statusBarHeight + 'px', background: statusBarBG }"
        ></view>
        <!-- 头部核心 -->
        <view
          class="page-header-nav"
          :style="{ height: headerHeight + 'px' }"
        >
          <view class="header-main">
            <slot name="gHeader"></slot>
          </view>
          <view :style="{ width: BoundingWidth, height: '10px' }"></view>
        </view>
      </view>
      <!-- 占位 view -->
      <view
        :style="{
          width: '100%',
          height: statusBarHeight + headerHeight + 'px'
        }"
      ></view>
    </view>
    <!-- 页面主体 -->
    <view class="page-body">
      <slot></slot>
      <slot name="gBody"></slot>
    </view>
    <!-- #ifndef APP-PLUS -->
    <view
      v-if="!isSwitchPage"
      :style="{ width: '100%', height: iphoneXButtomHeight + 'px' }"
    ></view>
    <!-- #endif -->
    <!-- 页面底部 -->
    <view
      class="page-footer"
      :style="{ 'z-index': footerIndex, background: footerBg }"
    >
      <slot name="gFooter"></slot>
      <!-- iphoneX 占位 view -->
      <!-- #ifndef APP-PLUS -->
      <view
        v-if="!isSwitchPage"
        :style="{ width: '100%', height: iphoneXButtomHeight + 'px' }"
      ></view>
      <!-- #endif -->
    </view>
    <!-- 右下角悬浮按钮 -->
    <view
      class="gui-page-rb-area"
      :style="{
        right: rbRight + 'rpx',
        bottom: rbBottom + 'rpx',
        width: rbWidth + 'rpx',
        zIndex: rbIndex
      }"
    >
      <slot name="gRTArea"></slot>
    </view>
    <!-- 全屏 loading -->
    <view
      class="page-loading"
      @tap.stop
      @touchmove.stop
      :style="{ background: loadingBG }"
      v-if="isLoading"
    >
      <view class="page-loading-point">
        <view
          class="page-loading-points animate1"
          :style="{ background: loadingPointBg }"
        ></view>
        <view
          class="page-loading-points animate2"
          :style="{ background: loadingPointBg }"
        ></view>
        <view
          class="page-loading-points animate3"
          :style="{ background: loadingPointBg }"
        ></view>
      </view>
    </view>
  </view>
</template>
<script>

export default {
  props: {
    customHeader: { type: Boolean, default: true },
    headerHeight: { type: Number, default: 44 },
    headerIndex: { type: Number, default: 1 },
    headerBG: { type: String, default: "none" },
    statusBarBG: { type: String, default: "none" },
    footerIndex: { type: Number, default: 1 },
    footerBg: { type: String, default: "" },
    isSwitchPage: { type: Boolean, default: false },
    rbWidth: { type: Number, default: 100 },
    rbBottom: { type: Number, default: 100 },
    rbRight: { type: Number, default: 20 },
    rbIndex: { type: Number, default: 1 },
    borderWidth: { type: String, default: "0" },
    borderColor: { type: String, default: "#D1D1D1" },
    loadingBG: { type: String, default: "rgba(255,255,255, 0.1)" },
    isLoading: { type: Boolean, default: false },
    loadingPointBg: { type: String, default: "#3688FF" }
  },
  data() {
    return {
      statusBarHeight: 0,
      iphoneXButtomHeight: 0,
      BoundingWidth: "0px"
    };
  },
  created: function() {
    try {
      var res = uni.getSystemInfoSync();
      res.model = res.model.replace(" ", "");
      res.model = res.model.toLowerCase();
      if (
        res.model.indexOf("iphonex") != -1 ||
        res.model.indexOf("iphone11") != -1
      ) {
        this.iphoneXButtomHeight = uni.upx2px(50);
      }
    } catch (e) {
      return null;
    }
    if (!this.customHeader) {
      return;
    }
    // #ifndef MP-ALIPAY
    this.statusBarHeight = res.statusBarHeight;
    // #ifdef MP-WEIXIN
    // 小程序胶囊按钮
    var res = uni.getMenuButtonBoundingClientRect();
    this.BoundingWidth = res.width + 20 + "px";
    // #endif
    // #endif
  }
};
</script>
<style>
/* #ifndef MP */
page {
  width: 100%;
  min-height: 100%;
  display: flex;
  flex-direction: column;
  flex: 1;
}
/* #endif */
.sbody {
  display: flex;
  flex-direction: column;
  width: 100%;
  min-height: 100%;
  flex: 1;
}
/* #ifdef MP */
.sbody {
  min-height: 100vh;
}
/* #endif */
.page-header {
  width: 100%;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99;
  border-bottom: 0px solid #ffffff;
}
.page-status-bar {
  width: 100%;
  height: 0;
}
.page-header-nav {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  height: 44px;
  align-items: center;
}
.header-main {
  width: 300rpx;
  flex: auto;
  overflow: hidden;
}
.page-body {
  width: 100%;
  flex: 1;
  display: flex;
  flex-direction: column;
}
.page-footer {
  width: 100%;
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 99;
}
.gui-page-rb-area {
  width: 100rpx;
  position: fixed;
  right: 20rpx;
  bottom: 100rpx;
  z-index: 1;
}

.page-loading {
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.1);
  position: fixed;
  z-index: 9999;
  left: 0;
  top: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.page-loading-point {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
}
.page-loading-points {
  width: 20rpx;
  height: 20rpx;
  background-color: #3688ff;
  border-radius: 50rpx;
  margin: 10rpx;
  opacity: 0.5;
}
@keyframes pageLoading1 {
  0% {
    opacity: 0.5;
    transform: scale(1);
  }
  40% {
    opacity: 1;
    transform: scale(1.5);
  }
  60% {
    opacity: 0.5;
    transform: scale(1);
  }
}
@keyframes pageLoading2 {
  20% {
    opacity: 0.5;
    transform: scale(1);
  }
  60% {
    opacity: 1;
    transform: scale(1.5);
  }
  80% {
    opacity: 0.5;
    transform: scale(1);
  }
}
@keyframes pageLoading3 {
  40% {
    opacity: 0.5;
    transform: scale(1);
  }
  80% {
    opacity: 1;
    transform: scale(1.5);
  }
  100% {
    opacity: 0.5;
    transform: scale(1);
  }
}
.animate1 {
  animation: pageLoading1 1.2s infinite linear;
}
.animate2 {
  animation: pageLoading2 1.2s infinite linear;
}
.animate3 {
  animation: pageLoading3 1.2s infinite linear;
}
</style>
