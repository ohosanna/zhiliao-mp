<script setup lang="ts">
definePage({
  name: "auth",
  style: {
    navigationBarTitleText: "",
    navigationStyle: "custom",
  },
});

const statusBarHeight = ref(20);
const router = useRouter();

// #ifdef MP-WEIXIN
const sysInfo = uni.getSystemInfoSync();
statusBarHeight.value = sysInfo.statusBarHeight || 20;
// #endif

function goBack() {
  uni.navigateBack();
}

const agreed = ref(false);

function toggleAgree() {
  agreed.value = !agreed.value;
}

const steps = [
  { name: "phone", label: "1" },
  { name: "register", label: "2" },
];

function getPhoneNumber(e: any) {
  // e.detail = { encryptedData, iv, cloudID }
  if (e.detail.errMsg && e.detail.errMsg.includes("ok")) {
    // TODO: 将 encryptedData/iv/cloudID 传到后端换取手机号
    uni.showToast({ title: "手机号获取成功", icon: "success" });
  } else {
    uni.showToast({ title: "已取消授权", icon: "none" });
  }
}

function nextStep() {
  if (!agreed.value) {
    uni.showToast({ title: "请先阅读并同意协议", icon: "none" });
    return;
  }
  // TODO: 进入下一步
  router.push({
    name: "register",
  });
}

function openAgreement() {
  // TODO: 打开用户服务协议
}

function openPrivacy() {
  // TODO: 打开隐私政策
}
</script>

<template>
  <view class="auth-page min-h-screen bg-gradient-to-b from-[#f0f4ff] to-white">
    <!-- 顶部返回 -->
    <view :style="{ paddingTop: `${statusBarHeight + 12}px` }">
      <view class="px-4">
        <view
          class="flex h-9 w-9 items-center justify-center rounded-full bg-white shadow-sm"
          @click="goBack"
        >
          <wd-icon name="arrow-left" size="18px" color="#374151" />
        </view>
      </view>
    </view>

    <!-- 标题区 -->
    <view class="mt-8 px-6 text-center">
      <text class="text-2xl font-bold wot-text-text-main"> 创建账号 </text>
      <text class="mt-2 block text-sm text-gray-500"> 注册后即可开启健康管理之旅 </text>
    </view>

    <!-- 步骤指示器 -->
    <view class="mt-8 flex items-center justify-center px-6">
      <view class="flex items-center">
        <view
          class="z-10 flex h-8 w-8 items-center justify-center rounded-full text-sm font-bold"
          :style="{
            backgroundColor: '#3b82f6',
            color: '#fff',
            boxShadow: '0 0 0 4px #dbeafe',
          }"
        >
          1
        </view>
        <view class="h-px w-12 bg-blue-500" />
        <view
          class="z-10 flex h-8 w-8 items-center justify-center rounded-full border text-sm font-medium"
          :style="{
            backgroundColor: '#fff',
            color: '#9ca3af',
            borderColor: '#e5e7eb',
          }"
        >
          2
        </view>
      </view>
    </view>

    <!-- 步骤标题 -->
    <view class="mt-6 px-6 text-center">
      <text class="text-base font-bold wot-text-text-main"> 手机号授权 </text>
      <text class="mt-2 block text-sm text-gray-500"> 小程序将自动获取您的手机号 </text>
    </view>

    <!-- 授权卡片 -->
    <view class="mx-4 mt-6 rounded-2xl bg-white p-6 shadow-sm">
      <!-- 手机图标 -->
      <view class="flex justify-center">
        <view class="mobile-icon flex h-16 w-16 items-center justify-center rounded-full">
          <i class="i-carbon-mobile w-8 h-8" />
        </view>
      </view>

      <text class="mt-4 block text-center text-sm text-gray-600">
        点击下方按钮，微信授权自动获取您的手机号
      </text>

      <!-- 微信授权按钮 -->
      <button
        class="auth-btn mt-5 flex h-11 w-full items-center justify-center gap-2 rounded-full border-0 text-base font-medium text-white"
        open-type="getPhoneNumber"
        @getphonenumber="getPhoneNumber"
      >
        <wd-icon name="wechat" size="18px" color="#fff" />
        <text class="text-base font-medium text-white"> 微信授权获取手机号 </text>
      </button>

      <!-- 协议勾选 -->
      <view class="mt-4 flex items-start justify-center" @click="toggleAgree">
        <view
          class="mt-0.5 flex h-4 w-4 shrink-0 items-center justify-center rounded"
          :style="{
            backgroundColor: agreed ? '#3b82f6' : '#fff',
            border: agreed ? 'none' : '1.5px solid #d1d5db',
          }"
        >
          <wd-icon v-if="agreed" name="check" size="12px" color="#fff" />
        </view>
        <text class="ml-2 text-xs leading-relaxed text-gray-500">
          <text>我已阅读并同意</text>
          <text class="text-blue-500" @click.stop="openAgreement"> 《用户服务协议》 </text>
          <text>和</text>
          <text class="text-blue-500" @click.stop="openPrivacy"> 《隐私政策》 </text>
        </text>
      </view>
    </view>

    <!-- 下一步按钮 -->
    <view class="px-4 pt-6">
      <wd-button type="primary" round block size="large" custom-class="next-btn" @click="nextStep">
        下一步
      </wd-button>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="30px" />
  </view>
</template>

<style lang="scss">
.auth-page {
  .mobile-icon {
    color: #22c55e;
    background: #dcfce7;
  }
  .auth-btn {
    background: #22c55e;
  }
  .next-btn {
    :deep(.wd-button) {
      border-radius: 999px;
      height: 48px;
      font-size: 16px;
      font-weight: 600;
    }
  }
}
</style>
